<properties 
    pageTitle="Suche Leistung und Optimierung Aspekte Azure | Microsoft Azure" 
    description="Optimieren Sie der Leistung von Azure suchen und konfigurieren optimale Skalierung" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure Suche Leistung und Optimierung Aspekte

Eine gute Suchfunktion ist ein Schlüssel zum Erfolg für viele Mobile und Webanwendungen. Von Immobilien verwendet Auto Marktplätze online Kataloge, wirkt schnelle Suche und relevante Ergebnisse der Customer Experience sich. Dieses Dokument soll Hilfe feststellen, dass Sie bewährte Methoden für die Suche besonders für erweiterte Szenarien mit anspruchsvolle Anforderungen für Skalierbarkeit, Azure mehrsprachigen optimal nutzen unterstützen und benutzerdefinierte Rangfolge.  Darüber hinaus dieses Dokument werden in Bezug auf die und Vorgehensweisen, die sowohl in apps praktisches Kunden behandelt.

## <a name="performance-and-scale-tuning-for-search-services"></a>Leistung und Optimieren von Maßstab für Dienste suchen

Wir werden alle verwendet, um die Suchmaschinen wie Bing und Google und die hohe Leistung, die sie bieten.  Daher, wenn Kunden Ihre Suche-fähigen Web oder mobilen Anwendung verwenden, erwartet vergleichbare Leistungsmerkmale.  Beim Optimieren für suchleistung ist eine der besten Vorgehensweisen konzentrieren der Wartezeit, also die Zeit, die eine Abfrage dauert durchführen und die Ergebnisse zurückgegeben.  Bei der Optimierung der Wartezeit bei der Suche ist es wichtig:

1. Wählen Sie eine Zielwartezeit (oder maximale Zeit), die eine typische Suche anfordern in Anspruch nehmen sollte.

2. Erstellen Sie und Testen Sie einer realen Arbeitsbelastung gegen Suchdienst mit einem realistischen Dataset messen, diese Wartezeit Sätze.

3. Beginnen Sie mit einer niedrigen Anzahl von Abfragen pro Sekunde (QPS), und erhöhen Sie die Anzahl in der Test ausgeführt wird, bis die Abfragewartezeit unter der Wartezeit definierten fällt weiterhin.  Hierbei handelt es sich um eine wichtige Erfolge helfen Ihnen für die Skalierung Planen Ihrer Anwendung zunehmender Verwendung.

4. Wann immer möglich, wiederverwenden Sie HTTP-Verbindungen.  Wenn Sie das .NET SDK Azure verwenden, bedeutet dies, sollten Sie wieder eine Instanz oder [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) Instanz verwenden, und wenn Sie die REST-API verwenden, sollten Sie eine einzelne HttpClient wiederverwenden.
 
Testen beim Erstellen der folgenden Auslastung, es gibt einige Merkmale Azure-Suche zu beachten:

1. Es ist möglich, Pushbenachrichtigungen, so viele suchen jeweils nur in Abfragen, die in Azure Suchdienst verfügbaren Ressourcen überfordert werden sollen.  In diesem Fall wird die Antwortcodes HTTP 503 angezeigt.  Aus diesem Grund empfiehlt es sich, beginnen Sie mit verschiedenen Bereichen von Suchabfragen die Unterschiede Wartezeit Sätzen angezeigt, während Sie weitere Suchabfragen hinzufügen.

2. Hochladen von Inhalten zu Azure wird die allgemeine Leistung und Wartezeit des Diensts Azure Suche auswirken.  Wenn Sie nicht vorhaben, Daten zu senden, während Benutzer suchen ausführen, ist es wichtig, diese Arbeitsbelastung berücksichtigt in Ihren Tests ausführen.

3. Nicht jeder Suchabfrage wird die gleiche Leistung Level ausgeführt.  Beispielsweise wird ein Vorschlag Dokument, Verweis oder Suchen in der Regel schneller als eine Abfrage mit einer erheblichen Anzahl Aspekte und Filter ausgeführt werden.  Es empfiehlt sich die verschiedenen Abfragen zu nutzen, die Sie nicht vorhaben, in Konto angezeigt werden soll, wenn die Tests zu erstellen.  

4. Variation Suchabfragen ist wichtig, da, wenn Sie die gleichen Suchabfragen ständig ausführen, Zwischenspeichern von Daten anfangen, vornehmen Leistung sehen besser als mit einer Abfrage mehr verschiedenartigen möglicherweise festlegen.

> [AZURE.NOTE] [Visual Studio laden testen](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) ist eine gute Möglichkeit Ihrer Benchmarktests ausführen, wie es können Sie HTTP-Anfragen auszuführen, wie Sie benötigen für Abfragen in Azure Suchen ausführen und ermöglicht die Parallelisierung Serviceanfragen.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Skalierung Azure nach hoher Abfrage Sätzen und Besprechungsanfragen gedrosselt

Wenn Sie zu viele reduzierten Anfragen Aktivierungsfehler oder Ihr Ziel Wartezeit Sätzen aus Laden einer Abfrage höhere überschreiten, können Sie Aussehen zum Verringern der Wartezeit Sätzen in einem der beiden folgenden Arten:

1. **Replikate vergrößern:**  Ein Replikat ist wie eine Kopie Ihrer Daten gleicht Azure-Suche zu Doppelraten-Anfragen anhand mehrerer Kopien zu laden.  Alle Lastenausgleich und Replikation von Daten in Replikaten von Azure suchen verwaltet wird und Sie können die Anzahl der Replikationen für Ihren Dienst vorgesehen sind, zu einem beliebigen Zeitpunkt ändern.  Sie können bis zu 12 Replikate in einer Standardansicht Suchdienst und der 3 in einen einfachen Suchdienst zuweisen.  Replikate können entweder aus dem [Azure-Portal](search-create-service-portal.md) oder mit dem [Azure-Suche Management-API](search-get-started-management-api.md)angepasst werden.

2. **Suche Ebene vergrößern:**  Azure Suche ist, in einer [Anzahl von Ebenen](https://azure.microsoft.com/pricing/details/search/) und jeder der folgenden Ebenen bietet verschiedene Detailebenen Leistung.  In einigen Fällen müssen Sie so viele Abfragen möglicherweise, dass die Ebene, die Sie auf ausreichend niedrig Wartezeit Sätzen, bereitstellen kann, selbst wenn sich Replikate überlastet sind.  In diesem Fall sollten Sie erwägen Sie die Nutzung von eine höhere Suche Ebenen wie der Azure Suche S3 Ebene, die für Szenarien mit einer großen Anzahl von Dokumenten und Abfrage extrem hoher Auslastung gut geeignet ist.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Anpassungsbereich für Azure nach langsam einzelne Abfragen

Ein weiterer Grund, warum Wartezeit Sätzen verlangsamten können, befindet sich in einer einzigen Abfrage zu lange ausführen.  Hinzufügen von Replikaten wird in diesem Fall nicht Wartezeit Sätzen verbessert.  Für diesen Fall stehen zwei Optionen zur Verfügung:

1. **Vergrößern von Partitionen** Eine Partition ist ein Verfahren für die Aufteilung von Daten auf zusätzliche Ressourcen.  Beim Hinzufügen einer zweiten Partition, erhält die Daten aus diesem Grund in zwei Teilen.  Eine dritte Partition teilt Ihre Index in drei usw. an.  Dies hat auch den Effekt, dass in einigen Fällen langsame Abfragen schneller aufgrund der Parallelisierung der Berechnung ausführen soll.  Es gibt einige Beispiele für die Stelle, an der gesehen haben diese Parallelisierung extrem gut mit Abfragen arbeiten, die Abfragen weisen eine geringe Selektivität aufweisen.  Diese besteht aus Abfragen, die viele Dokumente entsprechen, oder wenn muss Faceting zählt über großen Anzahl von Dokumenten zu ermöglichen.  Da es zahlreiche Berechnung erforderlich gibt, um die Relevanz der Dokumente bewerten oder zum zählen der Anzahl von Dokumenten, kann hinzufügen zusätzliche Partitionen dazu beitragen zusätzliche Berechnung angeben.  

   Maximal 12 Partitionen in Standard-Suchdienst und 1 Partition im grundlegende Suchdienst können vorhanden sein.  Partitionen können entweder aus dem [Azure-Portal](search-create-service-portal.md) oder mit dem [Azure Suche Management-API](search-get-started-management-api.md)angepasst werden.

2. **Hoher Kardinalität Felder beschränken:** Ein hoher Kardinalität Feld besteht aus einer Facetable oder gefiltert Feld, verfügt über eine signifikante Anzahl eindeutiger Werte, und daher akzeptiert viele Ressourcen, über die Ergebnisse zu berechnen.   Angenommen, würde wird eine Produkt-ID oder Beschreibung als Facetable/gefiltert für hohe Kardinalität, da die meisten Werte Dokuments Dokument eindeutig sind. Nach Möglichkeit, beschränken Sie die Anzahl der hohe Kardinalität Felder aus.

3. **Suche Ebene vergrößern:**  Bis zu verschieben, kann eine höhere Azure Suche Stufe eine weitere Möglichkeit zum Verbessern der Leistung von Abfragen langsam sein.  Jede höheren Ebene enthält auch schnellere CPU und mehr Arbeitsspeicher, der einen positiven Einfluss auf die abfrageleistung enthalten kann.

## <a name="scaling-for-availability"></a>Skalieren für Verfügbarkeit

Replikate nicht nur die Abfragewartezeit reduzieren, sondern können auch für eine hohe Verfügbarkeit zulassen.  Mit einem einzelnen Replikat erwarten regelmäßigen Ausfallzeiten aufgrund der Server neu gestartet wird nach Softwareupdates oder für andere Wartung Ereignisse, die stattfindet.  Daher ist es wichtig, zu beachten, wenn die Anwendung hohen Verfügbarkeit von Suchvorgängen (Abfragen) sowie schreibt (Indizierung Ereignisse) erforderlich ist.  Azure-Suche bietet Vereinbarung zum SERVICELEVEL-Optionen auf die bezahlte suchen Angebote mit den folgenden Attributen:

- 2 Replikate hohen Verfügbarkeit der schreibgeschützt Auslastung (Abfragen)
- 3 oder mehr Replikate hohen Verfügbarkeit der Lese-und Schreibzugriff Auslastung (Abfragen und Indizierung)

Weitere Informationen hierzu finden Sie auf der [Ebene Azure Suche Servicevertrag](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Da Replikate Kopien Ihrer Daten sind, kann mehrere Replikate Probleme Azure suchen, um nach einem Neustart und Wartung gegen eine Replikation nacheinander, während Sie Abfragen an die anderen Replikate ausgeführt werden weiterhin Computer.  Daher müssen Sie auch berücksichtigen, wie diese Ausfallzeiten die Abfragen beeinträchtigen, die nun gegen weniger eine Kopie der Daten ausgeführt werden müssen.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Skalierung Auslastung Geo verteilt und Geo-Redundanz bereitzustellen

Für Geo verteilt Auslastung finden Sie, dass Benutzer ansässig alles andere als das Data Center, wo Ihre Azure Suchdienst gehostet wird, höhere Wartezeit Raten hat.  Aus diesem Grund ist es häufig mehrere Suchdienste in Regionen haben, die in der Nähe näher diese Benutzer sind wichtig.  Azure Suche aktuell bietet keine automatisierte Methode zur Azure-Suche Indizes Geo Replikation über Regionen, aber es gibt einige Techniken, die verwendet werden können, die dieses Verfahren zum Implementieren und Verwalten von einfachen vornehmen können. Diese werden in den nächsten Abschnitten beschrieben.

Das Ziel einer Reihe Geo verteilt Dienste suchen ist in zwei oder mehr Regionen zwei oder mehr Indizes haben, in dem ein Benutzer mit dem Azure Suchdienst, die die niedrigste Wartezeit bereitstellt weitergeleitet werden, wie in diesem Beispiel wird gezeigt:

   ![Kreuztabellen-Dienste nach region][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Synchronisieren von Daten für mehrere Dienste von Azure-Suche

Es gibt zwei Optionen für die Ihrer verteilten Suche Dienste synchron die entweder unter Verwendung der [Azure Search Indexer](search-indexer-overview.md) oder die Pushbenachrichtigungen API (auch die [Azure Suche REST-API](https://msdn.microsoft.com/library/dn798935.aspx)genannt) bestehen aus.  

### <a name="azure-search-indexers"></a>Indexer Azure suchen

Wenn Sie die Azure Search Indexer verwenden, sind Sie bereits Daten geändert von einer zentralen Datenspeicher wie Azure SQL-DB oder DocumentDB importieren. Beim Erstellen einer neuen Suche Dienst Sie einfach auch Erstellen einer neuen Azure Search Indexer für diesen Dienst, die auf dieser dieselbe Datenspeicher verweist. Bei jedem neue Änderungen in den Datenspeicher stammen werden diese dann durch die verschiedenen Indexer indiziert.  

Hier ist ein Beispiel für diese Architektur aussehen würde.

   ![Einzelne Datenquelle mit verteilten Indexer und Dienst Kombinationen][2]


### <a name="push-api"></a>Pushbenachrichtigungen-API 
Wenn Sie Azure Suche Pushbenachrichtigungen API aktualisieren von [Inhalten in Azure Suchindex](https://msdn.microsoft.com/library/dn798930.aspx)verwenden, können Sie behalten Ihrer Dienste für verschiedene Suche synchron Änderungen auf alle Suchdienste drücken Sie bei jedem ein Update erforderlich ist.  Dabei ist es wichtig, um sicherzustellen, dass in Fällen, wo ein Update auf eine Suchdienst schlägt fehl, und ein oder mehrere Updates erfolgreich, zu behandeln.

## <a name="leveraging-azure-traffic-manager"></a>Nutzung von Azure Datenverkehr-Manager

[Azure Datenverkehr-Manager](../traffic-manager/traffic-manager-overview.md) können Sie die Weiterleitung von Anfragen zu mehreren Geo ansässig Websites, die dann von mehreren Azure Search Services unterstützt werden.  Ein Vorteil von Datenverkehr Manager ist, dass Azure suchen Prüfpunkt, um sicherzustellen, dass es zur Verfügung steht, und Benutzer an alternative Suche Services im Falle Ausfallzeiten weiterleiten.  Darüber hinaus können Azure Datenverkehr Manager, wenn Sie Suchabfragen bis Azure Websites weiterleiten, Saldo Fällen laden, wo finde ich die Website von, Ihnen aber nicht Azure suchen.  Hier ist ein Beispiel für welche die Architektur, die Datenverkehr Manager nutzt.

   ![Kreuztabellen-Dienste nach Region, mit zentralen Datenverkehr Manager][3]

## <a name="monitoring-performance"></a>Überwachen der Leistung

Azure-Suche bietet die Möglichkeit, analysieren und Überwachen der Leistung des Diensts durch [Suchen den Datenverkehr Analytics (STA)](search-traffic-analytics.md). Durch STA können Sie optional einzelne Suchvorgängen als auch zusammengefasster Kennzahlen mit einer Firma Azure-Speicher protokollieren, die für die Analyse verarbeitet oder in Power BI visualisiert werden können.  STA Kennzahlen können Sie über die durchschnittliche Anzahl von Abfragen oder Reaktionszeiten auf Abfragen Leistungsstatistiken überprüfen.  Darüber hinaus können mit der Protokollierung der Vorgang Sie Ausführen von Drilldowns oder in bestimmten Suchvorgängen Details.

STA ist ein wertvolles Tool zu verstehen Wartezeit Sätzen aus dieser Perspektive Azure suchen.  Da die Abfrage Performance-Werte angemeldet auf die Uhrzeit basieren, die eine Abfrage verarbeitet wird, werden vollständig in Azure-Suche (ab dem Zeitpunkt, dem dies erforderlich ist, wenn es sich gesendet wird), können Sie diese verwenden, um festzustellen, ob Wartezeitprobleme aus der Suche Azure Service Seite oder außerhalb der Dienst, beispielsweise von Netzwerkwartezeit sind.  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Preisen Ebenen und Diensten Grenzwerte für jeden finden Sie unter [Service Grenzwerte in Azure suchen](search-limits-quotas-capacity.md).

Besuchen Sie [Planen Kapazität](search-capacity-planning.md) , erfahren Sie mehr über Partition und Kopie Kombinationen aus.

Für weitere Drilldowns auf die Systemleistung und um festzustellen, einige vorgeführt, wie die in diesem Artikel beschriebenen Optimierungen implementiert wird können ansehen Sie folgende Video:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png