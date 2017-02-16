<properties
   pageTitle="Verteilen von Daten mit DocumentDB Global | Microsoft Azure"
   description="Informationen Sie zu weltweit-Skala Geo-Replikation, Failover und Daten Wiederherstellung globale Datenbanken aus Azure DocumentDB, eine vollständig verwaltete NoSQL-Datenbank-Dienst verwenden."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Verteilen von Daten mit DocumentDB Global

> [AZURE.NOTE] Globale Verteilung der DocumentDB Datenbanken ist in der Regel verfügbar und für alle neu erstellten DocumentDB Konten automatisch aktiviert. Wir arbeiten an globale Verteilung auf alle vorhandenen Konten aktivieren, aber in der Zwischenzeit Bedarf globale Verteilerlisten auf Ihr Konto aktiviert wenden Sie sich bitte [an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , und wir werden Aktivieren der Arbeitsmappe für Sie jetzt.

Azure DocumentDB soll den Anforderungen der IoT Applications aus mehreren Millionen Global verteilten Geräte und Internet-Skala Applications, die Benutzer in der ganzen Welt hochgradig reagiert Erfahrung vorführen. Diese Datenbanksysteme mit der Herausforderung erreichen niedriger Wartezeit Zugriff auf Anwendungsdaten aus mehreren geographischen Regionen mit klar definierten Daten Absturz Garantien konfrontiert. Wie ein System Global verteilten Datenbank vereinfacht DocumentDB die globale Verteilung der Daten durch das Angebot vollständig verwaltete Datenbank mit mehreren Konten, die löschen Kompromisse zwischen Konsistenz, Verfügbarkeit und Leistung, alle mit entsprechenden Garantien bereitstellen. DocumentDB Datenbank-Konten mit hohen Verfügbarkeit, einzelne Ziffer ms Wartezeiten, mehrere [Ebenen klar definierte Konsistenz]erhältlichen [consistency], transparent Landes-/ Failover mit Multi-homing APIs und elastisch Durchsatz und Speicher des Globus skalieren. 

  
## <a name="configuring-multi-region-accounts"></a>Konfigurieren von mit mehreren Konten

Konfigurieren Ihr Konto DocumentDB zum Skalieren des Globus kann in weniger als einer Minute über das [Azure-Portal](documentdb-portal-global-replication.md)vorgenommen werden. Sie müssen lediglich wählen Sie die richtigen Konsistenz zwischen mehreren unterstützten klar definierte Konsistenz Ebenen, und ordnen Sie eine beliebige Anzahl von Azure Regionen mit Ihrem Datenbankkonto. DocumentDB Konsistenz Ebenen bieten löschen Kompromisse zwischen bestimmten Konsistenz Garantie und Leistung. 

![Definiert die DocumentDB bietet mehrere, auch (niedrige) Konsistenz-Modelle zur Auswahl][1]

DocumentDB bietet mehrere, gut definierte (niedrige) Konsistenz-Modelle zur Auswahl.

Auswählen der richtigen Konsistenz Ebene, hängt davon ab, Daten Konsistenz sichergestellt ist, Ihren Anforderungen Anwendung. DocumentDB automatisch die Daten in allen angegebenen Regionen repliziert und garantiert die Konsistenz, die Sie für Ihr Datenbankkonto ausgewählt haben. 


## <a name="using-multi-region-failover"></a>Verwenden mehrere Region failover 

Azure DocumentDB ist transparent Failover Datenbankkonten können über mehrere Azure Regionen – die neuen [Multi-homing APIs] [ developingwithmultipleregions] Garantie, dass Ihre app kann weiterhin einen logischen Endpunkt verwenden und nicht durch das Failover unterbrochen. Failover wird gesteuert, indem Sie, bietet die Flexibilität, zu Ihrer Datenbank verlagern-Konto in das Ereignis Teile des Textbereichs Bedingungen möglicherweise Fehler auftreten, einschließlich der Anwendung, Infrastruktur, Dienst oder regionalen Fehlern (real oder simulierten). Bei einem DocumentDB regionalen Ausfall der Dienst nicht transparent über Ihr Datenbankkonto und Ihrer Anwendung weiterhin auf Daten zugreifen können, ohne Verlust der Verfügbarkeit. Während der DocumentDB [99,99 % Verfügbarkeit SLAs]bietet[sla], Sie können Ihrer Anwendung Ende, um die Verfügbarkeit von Eigenschaften Ende testen, indem Sie die Landes-/ ein Fehler beim beide, [programmgesteuert] simulieren[ arm] ebenso wie über das Azure-Portal.


## <a name="scaling-across-the-planet"></a>Skalierung über weltweit
DocumentDB können Sie unabhängig voneinander Durchsatz bereitstellen und nutzen Speicherplatz für jede Websitesammlung DocumentDB bei einer beliebigen Skalierung, global über alle Bereiche, die mit Ihrem Datenbankkonto verknüpft ist. Eine Auflistung DocumentDB automatisch Global verteilt und verwaltet über alle Bereiche, die mit Ihrem Datenbankkonto verknüpft ist. Websitesammlungen in Ihr Datenbankkonto können auf eine der in der Azure Regionen verteilt werden die [DocumentDB Dienst steht][serviceregions]. 

Die Durchsatz erworben haben und für jede Websitesammlung DocumentDB verbraucht Speicher wird automatisch bereitgestellt für alle Regionen gleichmäßig. Auf diese Weise können nahtlos über der Globus [bezahlen nur für die Durchsatz und Speicher innerhalb jeder Stunde verwendetem]Skalierung die Anwendung,[pricing]. Z. B., wenn Sie 2 Millionen RUs für eine Websitesammlung DocumentDB bereitgestellt haben, erhält dann den Regionen, die mit Ihrem Datenbankkonto verbundene 2 Millionen RUs für diese Websitesammlung. Dies ist unten dargestellt.

![Anpassungsbereich für Durchsatz für eine Websitesammlung DocumentDB über die vier Bereiche][2]

DocumentDB garantiert < 10 ms Lesen und < 15 ms Schreiben Wartezeiten bei P99. Die gelesen Anfragen erstrecken nie Datacenter-Seitenrand, um die [Konsistenz-Anforderungen, die Sie ausgewählt haben]sichergestellt ist[consistency]. Die schreibt sind immer Quorum zugesichert lokal, bevor sie an die Clients bestätigt werden. Jedes Datenbankkonto wird mit Schreiben Region Priorität konfiguriert. Die Region gekennzeichnet mit höchster Priorität fungiert als aktuellen Bereich Schreiben für das Konto. Alle SDKs werden transparent Datenbank Konto schreibt den aktuell schreiben Bereich weitergeleitet. 

Schließlich, da DocumentDB vollständig [Schema-unabhängige] ist[ vldb] -nie Schemas verwalten/aktualisieren oder sekundäre Indizes für mehrere Rechenzentren kümmern müssen. Der [SQL-Abfragen] [ sqlqueries] weiterhin arbeiten, während Ihrer Anwendung und Datenmodelle weiterhin weiterentwickelt. 


## <a name="enabling-global-distribution"></a>Aktivieren der globalen Verteilung 

Entscheiden, dass die Daten lokal oder Global verteilt, indem Sie entweder zuordnen möchten Sie eine oder mehrere Azure Regionen mit einer DocumentDB Datenbank-Konto. Sie können hinzufügen oder Entfernen von Regionen bei Ihrem Datenbankkonto zu einem beliebigen Zeitpunkt. Zum Aktivieren mithilfe des Portals globale Verteilerlisten finden Sie unter [globale Datenbankreplikation DocumentDB über das Azure-Portal durchführen](documentdb-portal-global-replication.md). Um globale Verteilerlisten programmgesteuert zu aktivieren, finden Sie unter [Entwickeln mit mehreren Region DocumentDB Konten](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verteilen von Daten Global mit DocumentDB in den folgenden Artikeln:

* [Bereitstellung von Durchsatz und Speicherplatz für eine Websitesammlung] [throughputandstorage]
* [Multi-homing APIs über REST. .NET, Java, Python und Knoten SDKs] [developingwithmultipleregions]
* [Konsistenz Ebenen in DocumentDB] [consistency]
* [Verfügbarkeit von SLAs] [sla]
* [Verwalten von Datenbankkonto] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

