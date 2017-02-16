<properties
    pageTitle="Abfrage Azure Suchindex | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Erstellen Sie eine Suchabfrage in Azure suchen und Verwenden von Suchparametern zu filtern und Sortieren von Suchergebnissen."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>Abfrage Azure Suchindex
> [AZURE.SELECTOR]
- [(Übersicht)](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Wenn Suchabfragen zu Azure gesendet werden, gibt es eine Reihe von Parametern, die entlang der tatsächlichen Wörter angegeben werden, die in das Suchfeld der Anwendung eingegeben werden. Diese Abfrageparameter können Sie einige tieferen Kontrolle über die voll-Textsuche unterwegs zu erzielen.

Es folgt eine Liste, die gängige Verwendungszwecke der Abfrageparameter in Azure Suche kurz erläutert. Vollständige Schutz der Abfrageparameter und deren Verhalten finden Sie ausführlichen Seiten für die [REST-API](https://msdn.microsoft.com/library/azure/dn798927.aspx) und [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Typen von Abfragen

Azure-Suche bietet viele Optionen zum äußerst leistungsfähige Abfragen zu erstellen. Abfrage Sie verwenden die zwei Hauptarten sind `search` und `filter`. A `search` Abfrage für einen oder mehrere Ausdrücke in alle _durchsuchbare_ Felder in Ihrem Index durchsucht, und wie eine Suchmaschine wie Google oder Bing entwickelt würde erwartet funktioniert. A `filter` Abfrage wertet einen booleschen Ausdruck über alle _gefiltert_ Felder in einem Index. Im Gegensatz zu `search` Abfragen `filter` Abfragen entsprechen den genauen Inhalt eines Felds, d. h., sie sind für Zeichenfolgenfelder Groß-/Kleinschreibung.

Suchen und Filtern können einzeln oder zusammen verwendet werden. Wenn Sie diese gemeinsam verwenden, der Filter für den gesamten Index zuerst angewendet wird, und klicken Sie dann die Suche auf die Ergebnisse des Filters ausgeführt wird. Filter können daher eine nützliche Technik für optimale Leistung, da sie die Menge der Dokumente reduzieren, die Suchabfrage verarbeiten muss, Abfrage sein.

Die Syntax für Filterausdrücke ist eine Teilmenge der [Sprache für OData-Filter](https://msdn.microsoft.com/library/azure/dn798921.aspx). Für Suchabfragen können Sie entweder die [vereinfachte Syntax](https://msdn.microsoft.com/library/azure/dn798920.aspx) oder die [Abfragesyntax Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) die weiter unten behandelt werden.

### <a name="simple-query-syntax"></a>Die Syntax der Auswahlabfrage
Die [Syntax der Auswahlabfrage](https://msdn.microsoft.com/library/azure/dn798920.aspx) ist der Standard-Abfragesprache in Azure Suchen verwendet werden. Die Syntax der Auswahlabfrage unterstützt eine Anzahl von Allgemeine Suche Operatoren "und", einschließlich der oder nicht, Satz, Suffix und der Rangfolge Operatoren.

### <a name="lucene-query-syntax"></a>Lucene Abfragesyntax
Die [Abfragesyntax Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) können Sie als Teil des [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)entwickelte ausweitet und ausdrucksbasierte Abfragesprache verwenden.

Mithilfe dieser Abfragesyntax können Sie die folgenden Funktionen leicht zu erreichen: [Feld ausgelegte Abfragen](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [fuzzy-Suche](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [Suche](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [Ausdruck verstärken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [Suche mit regulären Ausdrücken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [Platzhalterzeichen suchen](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [Syntax Grundlagen](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)und [Abfragen mithilfe von boolesche Operatoren](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Sortierung der Ergebnisse
Wenn Ergebnisse für eine Suchabfrage empfangen, können Sie Azure Suche die Ergebnisse sortiert nach Werten in einem bestimmten Feld fungiert anfordern. Standardmäßig Bestellungen Azure Suchen der Suchergebnisse basierend auf den Rang des Dokuments suchen Bewertung von [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)abgeleitet wird.

Wenn Azure Suche zurückgegeben, die Ergebnisse sortiert werden durch einen anderen Wert als die Bewertung suchen können soll die `orderby` Parameter suchen. Sie können angeben, dass den Wert von der `orderby` Parameter enthalten Feldnamen und Anrufe an die [ `geo.distance()` (Funktion)](https://msdn.microsoft.com/library/azure/dn798921.aspx) für Geodaten Werte. Jeder Ausdruck folgen kann `asc` um anzugeben, dass die Ergebnisse in aufsteigender Reihenfolge, angefordert werden und `desc` um anzugeben, dass die Ergebnisse in absteigender Reihenfolge angefordert werden. Die standardmäßige Rangfolge aufsteigender Reihenfolge.

## <a name="paging"></a>Seitennavigation
Azure Suche erleichtert die Seitennavigation von Suchergebnissen zu implementieren. Mithilfe der `top` und `skip` Parameter, können Sie störungsfrei Suchabfragen, mit denen Sie die gesamte Sammlung von Suchergebnissen in überschaubare, Ziffern oder Buchstaben sortierte Untergruppen zu erhalten, die einfach gute suchen UI Methoden unterstützen ausgeben. Wenn diese eine kleinere Teilmenge Ergebnisse zu empfangen, können Sie auch die Anzahl von Dokumenten in der gesamten Satz von Suchergebnissen erhalten.

Sie können weitere Informationen zur Auslagerungsdatei Suchergebnisse im Artikel [So Seite Suchergebnisse in Azure suchen](search-pagination-page-layout.md).


## <a name="hit-highlighting"></a>Drücken Sie die Hervorhebung
In Azure suchen, Hervorheben von Suchergebnissen, die die Suchabfrage entsprechen den genauen Teil besteht einfach mithilfe der `highlight`, `highlightPreTag`, und `highlightPostTag` Parameter. Sie können angeben, welche _durchsuchbare_ Felder deren übereinstimmende Text hervorgehoben haben sollten, sowie das angeben, gibt die genaue Zeichenfolge Tags, am Anfang und Ende des Texts abgeglichene, dass die Suche Azure angefügt werden soll.
