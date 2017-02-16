<properties
    pageTitle="Lucene Abfragebeispiele für die Suche Azure | Microsoft Azure-Suche"
    description="Lucene Abfragesyntax für fuzzy-Suche, Suche, Ausdruck verstärken, Suche mit regulären Ausdrücken und Platzhalterzeichen suchen."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene Abfrage Syntaxbeispiele für das Erstellen von Abfragen in Azure-Suche

Beim Erstellen von Abfragen für Azure suchen, können Sie entweder die [einfache Abfragesyntax](https://msdn.microsoft.com/library/azure/dn798920.aspx) oder der alternative [Lucene Abfrage Parser in Azure suchen](https://msdn.microsoft.com/library/azure/mt589323.aspx). Der Abfrage-Parser Lucene unterstützt komplexere Abfragekonstrukte, wie Feld ausgelegte Abfragen, fuzzy-Suche, Suche, Ausdruck verstärken und Reqular Ausdruck suchen.

In diesem Artikel können Sie Beispiele schrittweise, die Abfragesyntax Lucene und Ergebnisse nebeneinander anzeigen. Führen Sie Beispiele für eine vorab geladen Suchindex in [JSFiddle](https://jsfiddle.net/), einen online Code-Editor für Tests Skripts und HTML. 

Mit der rechten Maustaste auf die Abfrage Beispiel URLs JSFiddle in einem eigenen Browserfenster geöffnet.

> [AZURE.NOTE] In den folgenden Beispielen Nutzung einen Suchindex Aufträge verfügbar basierend auf ein Dataset durch den [Ort der New York OpenData](https://nycopendata.socrata.com/) Initiative bereitgestellten aus. Diese Daten sollten nicht aktuelle oder eine vollständige angesehen werden. Der Index ist auf Sandkasten-Dienste von Microsoft. Ein Azure-Abonnements oder von Azure suchen, die Sie probieren diese Abfragen ist nicht erforderlich.

## <a name="viewing-the-examples-in-this-article"></a>Die Beispiele anzeigen in diesem Artikel

Alle in den Beispielen in diesem Artikel angeben der Lucene Abfrage Parser über den Parameter**QueryType** suchen. Bei Verwendung der Lucene Abfrage Parser Code erhalten Sie bei jeder Anforderung der **QueryType** angeben.  Gültige Werte sind **einfache**|**vollständige**, mit **einfachen** als Standard und **vollständigen** für den Abfrage-Parser Lucene. Details zum Angeben der Abfrageparameter finden Sie unter [Dokumente durchsuchen (Azure Suche Dienst REST API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

**Beispiel 1** – mit der rechten Maustaste die folgende Codeausschnitt Abfrage, damit es in eine neue Browserseite geöffnet, die JSFiddle lädt und führt die Abfrage aus:
- [& QueryType = vollständig & Suchen = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Diese Abfrage gibt Dokumente aus unserem Aufträge Index (auf einem geschützten Dienst geladen)

In dem neuen Browserfenster sehen Sie JavaScript Quell- und HTML-Ausgabe nebeneinander. Das Skript verweist auf eine Abfrage, die durch die URLs Beispiel in diesem Artikel bereitgestellt wird. Beispielsweise ist in **Beispiel 1**, die zugrunde liegende Abfrage wie folgt:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Beachten Sie die Abfrage verwendet einen vorkonfigurierten Azure Suchindex Nycjobs bezeichnet. Der Parameter **SearchFields** beschränkt die Suche auf nur Business Titelfeld an. Die **QueryType** ist auf **vollständige**, festgelegt angewiesen, Azure Suchen der Lucene Abfrage Parser für diese Abfrage verwendet werden soll.

### <a name="fielded-query-operation"></a>Mit denen sich Abfragevorgang

Sie können die Beispiele in diesem Artikel durch Angabe einer Konstruktion **Fieldname:searchterm** zum Definieren von einem Abfragevorgang mit denen sich, wo das Feld ist ein einzelnes Wort, und der Suchbegriff ist ebenfalls ein einzelnes Wort oder einen Begriff, optional mit booleschen Operatoren ändern. Einige Beispiele für Folgendes:

- Business_title:(senior NOT junior)
- Bundesstaat: ("Berlin" und "Neue Jersey")

Achten Sie darauf, dass Sie mehrere Zeichenfolgen in Anführungszeichen setzen, wenn beide Zeichenfolgen als einzelne Einheit, wie in diesem Fall nach zwei unterschiedlichen Orte in das Feld "Speicherort" ausgewertet werden sollen. Darüber hinaus muss der Operator wird groß geschrieben, wie Sie mit nicht sehen und and.

Das Feld angegeben **Fieldname:searchterm** muss ein Feld durchsucht. Details zur Verwendung von Indexattribute im Felddefinitionen finden Sie unter [Create Index (Azure Suche Dienst REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Fuzzy-Suche

Eine fuzzy-Suche findet Treffer in Ausdrücke, die eine ähnliche Konstruktion aufweisen. Pro [Lucene Dokumentation](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)basiert auf [Damerau-Levenshtein Abstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance)fuzzy Suchbegriffe.

Verwenden Sie zum Ausführen einer fuzzy-Suche das Tilden "~" Symbol am Ende ein einzelnes Wort mit einer optionalen Parameter, die einen Wert zwischen 0 und 2, die angibt, den Abstand bearbeiten. Beispielsweise "blauen ~" oder "Blau ~ 1" würde zurückgeben Blau, Blau und Kleben.

**Beispiel 2** – mit der rechten Maustaste im folgenden Abfrage Codeausschnitt zu probieren Sie es. Diese Abfrage sucht nach Business Titel mit den Begriff leitende in diese jedoch nicht stellvertretenden:

- [& QueryType = vollständig & Suchen = Business_title:senior nicht stellvertretenden](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Suche

Näherung Suchbegriffe werden verwendet, um die Begriffe zu suchen, die in einem Dokument benachbart sind. Einfügen einer Tilden "~" Symbol am Ende eines Satzes gefolgt von der Anzahl der Wörter, die die Begrenzungslinie Näherung erstellen. Beispielsweise "Hotel Airport" ~ 5 suchen die Begriffe Hotel und Airport innerhalb von 5 Wörtern von miteinander in einem Dokument.

**Beispiel 3** – mit der rechten Maustaste im folgenden Abfrage Codeausschnitt. Diese Abfrage sucht nach Einzelvorgänge mit dem der Ausdruck zuordnen (wobei es falsch geschrieben wurde):

- [& QueryType = vollständig & Suchen = Business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Beispiel 4** – der rechten Maustaste auf die Abfrage. Suchen Sie nach Aufträgen mit dem Begriff "leitenden Analysten", wo sie durch nicht mehr als ein Wort getrennt ist:

- [& QueryType = vollständig & Suchen = Business_title: "leitenden Analysten" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Beispiel 5** – Probieren Sie es erneut die Wörter zwischen den Begriff "leitenden Analysten" entfernen.

- [& QueryType = vollständig & Suchen = Business_title: "leitenden Analysten" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Laufzeit steigern

Ausdruck verstärken bezeichnet Rangfolgen eines Dokuments höher, wenn sie den Begriff stärkere relativ zu Dokumenten enthält, die den Ausdruck nicht enthalten. Dies unterscheidet sich von Profilen bewerten, dieses Bewertungssystem Profile bestimmte Felder, anstatt bestimmten Ausdrücken steigern. Im folgende Beispiel können Sie die Unterschiede zu veranschaulichen.

Beachten Sie, dass ein Bewertungssystem Profil, das steigert die in einem bestimmten Feld, wie z. B. **Genre** im Beispiel Musicstoreindex entspricht. Ausdruck verstärken könnte weiteren bestimmte Suche Ausdrücke als andere höhere steigern verwendet werden. Beispielsweise "Rock ^ 2 elektronische" wird durch fördern Dokumente, die die Suchbegriffe im Feld **Genre** größer als die anderen durchsuchbare Felder im Index enthalten. Darüber hinaus werden Dokumente, die den Suchbegriff "Rock" enthalten höher, als die anderen Suchbegriff "elektronische" als Ergebnis der Begriff steigern Value (2) eingestuft werden.

Durch einen Ausdruck fördern, verwenden Sie das Caretzeichen "^", Symbol steigern der Faktor (eine Zahl) am Ende des Ausdrucks, die Sie suchen. Je höher der steigern Faktor, desto relevanter wird der Begriff relativ zu anderen Suchbegriffe sein. Standardmäßig ist der steigern Faktor 1 auf. Obwohl der steigern Faktor positiv sein muss, kann das kleiner als 1 ist (z. B. 0,2) sein.

**Beispiel 6** – der rechten Maustaste auf die Abfrage. Suchen Sie nach der Einzelvorgänge mit dem Begriff "Computer Analysten", in dem erläutert wird, es gibt keine Ergebnisse mit Wörter Computer und für Analysten, aber Analysten Aufträge werden am oberen Rand der Ergebnisse.

- [& QueryType = vollständig & Suchen = Business_title:computer Analysten](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Beispiel 7** – versuchen Sie es erneut, dieses Mal verstärken mit dem Begriff Computer über den Begriff Analysten ergibt, wenn beide Wörter nicht vorhanden sind.

- [& QueryType = vollständig, und suchen Sie Business_title:computer = ^ 2 Analysten](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Reguläre Ausdrücke

Eine Suche mit regulären Ausdrücken findet eine Übereinstimmung basierend auf dem Inhalt zwischen Schrägstriche "/" als dokumentierten in der [RegExp-Klasse](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html)an.

**Beispiel 8** – der rechten Maustaste auf die Abfrage. Aufträge entweder mit Suchen Sie den Ausdruck leitende oder Junior.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Die URL für den in diesem Beispiel werden auf der Seite nicht ordnungsgemäß gerendert. Problem zu umgehen kopieren Sie die folgenden URL, und fügen Sie ihn in das Browser-URL-Adresse:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Suchen von Platzhalterzeichen

Sie können im Allgemeinen erkannten Syntax für mehrere (\*) oder ein einzelnes Zeichen-Platzhaltersuchen (?). Hinweis Die Lucene Abfrage Parser unterstützt die Verwendung dieser Symbole mit einem einzelnen Ausdruck und nicht in einem Satz.

**Beispiel 9** – der rechten Maustaste auf die Abfrage. Suchen Sie nach Aufträgen, die das Präfix 'Programm' also auch Business Titel mit den Konditionen programming und Programmierer darin enthalten.

- [& QueryType = vollständigen & $select = Business_title & Suche = Business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Sie können keine verwenden eine * oder? Symbol als erstes Zeichen einer Suche.


## <a name="next-steps"></a>Nächste Schritte

Testen der Abfrage-Parser Lucene im Code angeben. Die folgenden Links wird erläutert, wie Sie Suchabfragen für .NET und die REST-API einrichten. Die Links verwenden Sie die einfachen Standardsyntax, sodass Sie werden aus den in diesem Artikel, um anzugeben, die **QueryType**gelernten anwenden müssen.

- [Abfrage mit dem .NET SDK Azure Suchindex](search-query-dotnet.md)
- [Verwenden die REST-API Azure Suchindex Abfragen](search-query-rest-api.md)


 