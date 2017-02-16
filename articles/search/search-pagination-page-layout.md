<properties 
    pageTitle="Wie auf der Suchergebnisseite in Azure suchen | Microsoft Azure | Cloud gehosteten Suchdienst" 
    description="Seitenumbruch in einen gehosteten Cloud Suchdienst auf Microsoft Azure Azure Suche." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

#<a name="how-to-page-search-results-in-azure-search"></a>Wie Sie in Azure Suchen der Suchergebnisseite#

Dieser Artikel enthält Hinweise zur Verwendung der Azure Suche REST API standard Elemente einer Suchergebnisseite, wie z. B. Summe, Anzahl, Abruf von Dokumenten, Sortierreihenfolgen und Navigation implementiert wird.
 
In jedem Fall unten aufgeführten angegeben sind, Optionen im Zusammenhang mit der Seite, die Daten oder Informationen zu Ihrer Seite mit Suchergebnissen beitragen durch das [Dokument durchsuchen](http://msdn.microsoft.com/library/azure/dn798927.aspx) Anfragen an Ihre Azure Search-Dienst gesendet werden. Anfragen enthalten GET-Befehl, Pfad, und Abfrageparameter, die den Dienst zu informieren, was angefordert wird, und wie die Antwort zu erstellen.

> [AZURE.NOTE] Eine gültige Anforderung umfasst eine Anzahl von Elementen, wie etwa einen Dienst-URL und den Pfad, HTTP-Verb, `api-version`und so weiter. Aus Platzgründen abgeschnitten wir die Beispiele, um nur die Syntax zu markieren, die für den Seitenumbruch relevant sind. Finden Sie in der Dokumentation [Azure suchen Dienst REST-API](http://msdn.microsoft.com/library/azure/dn798935.aspx) Details der Anforderungssyntax.

## <a name="total-hits-and-page-counts"></a>Gesamtanzahl der Treffer und wird unabhängig von der Seite ##

Mit der Gesamtzahl der Ergebnisse einer Abfrage zurückgegeben werden soll, und klicken Sie dann die Ergebnisse zurückgegeben werden in kleineren Segmenten ist jedoch grundsätzliche auf nahezu allen Seiten der Suche.

![][1]
 
In Azure suchen, verwenden Sie die `$count`, `$top`, und `$skip` Parameter diese Werte zurückgegeben werden. Das folgende Beispiel zeigt eine Beispiel für eine Anforderung für insgesamt Treffer, zurückgegeben als `@OData.count`:

        GET /indexes/onlineCatalog/docs?$count=true

Abrufen von Dokumenten in Gruppen von 15, und auch die Gesamtanzahl der Treffer, beginnend mit der ersten Seite anzeigen:

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

Ergebnisse Paginieren benötigt diese beiden `$top` und `$skip`, wobei `$top` gibt an, wie viele Elemente in einem Stapel, Rückkehr und `$skip` gibt an, wie viele Elemente überspringen. Im folgenden Beispiel jeweils die inkrementell Sprünge in erkennbar Seite zeigt die nächsten 15 Elemente, die `$skip` Parameter.

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a>Layout  ##

Klicken Sie auf eine Seite mit Suchergebnissen sollten Sie eine Miniaturansicht, eine Teilmenge der Felder und einen Link zu einer Seite Vollversion anzeigen.

 ![][2]
 
In Azure suchen, verwenden Sie `$select` und Nachschlagen der Befehl diese Erfahrung zu implementieren.

Eine Teilmenge der Felder für eine nebeneinander angeordnete Layout zurück:

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

Bilder und Mediendateien sind nicht direkt durchsucht und in einer anderen Speicherplattform, wie z. B. Azure Blob-Speicher, um die Verwaltungskosten gespeichert werden soll. Definieren Sie in den Index und Dokumenten: ein Feld, in dem die URL-Adresse des externen Inhalts gespeichert. Sie können das Feld dann als ein Bezug Bild verwenden. Die URL zu dem Bild sollte in das Dokument.

Rufen Sie die Seite eines Produkts Beschreibung für ein Ereignis **OnClick** verwenden Sie [Nachschlagen Dokument](http://msdn.microsoft.com/library/azure/dn798929.aspx) der Schlüssel des Dokuments zum Abrufen von übergeben. Der Datentyp des Schlüssels ist `Edm.String`. In diesem Beispiel ist *246810*. 
   
        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a>Sortieren nach Relevanz, Bewertung oder Kurs ##

Sortierreihenfolgen häufig Standard zu erzielen, aber es sieht allgemeine alternative sortieren Bestellungen Flexibilität zur Verfügung zu stellen, damit Kunden schnell vorhandene Ergebnisse in einer anderen Rang Reihenfolge Zuordnung können.

 ![][3]

In Azure suchen, Sortierung basiert auf der `$orderby` Ausdruck für alle Felder, die als indiziert werden`"Sortable": true.`

Relevanz ist bewerten Profile stark zugeordnet. Können die standardmäßigen bewerten, die basiert auf der Text Analyse und Statistik Reihenfolge alle Ergebnisse, wobei höhere Werte vertraut zu Dokumenten mit weitere oder sicheres Treffer auf einen Suchbegriff Rangfolge von.

Alternative Sortierreihenfolgen, die in der Regel **OnClick** Ereignisse, die an eine Methode zurückzurufen, bei dem die Sortierreihenfolge erstellt zugeordnet sind. Betrachten Sie diese Seitenelements:

 ![][4]

Sie möchten eine Methode erstellen, die die ausgewählten Sortieroption als Eingabe akzeptiert und gibt eine sortierte Liste für die Kriterien, die mit dieser Option verbundene.

 ![][5]
 
> [AZURE.NOTE] Während der standardmäßigen Punktzahl für viele Szenarios ausreichend ist, wird empfohlen, ob Relevanz stattdessen auf ein benutzerdefiniertes Punktzahl Profil basiert. Ein benutzerdefiniertes Punktzahl Profil bietet Ihnen eine Möglichkeit zum steigern Elemente, die für Ihr Geschäft sinnvoll sind. Weitere Informationen finden Sie unter [Hinzufügen eines Profils Punktzahl](http://msdn.microsoft.com/library/azure/dn798928.aspx) . 

## <a name="faceted-navigation"></a>Facettierten navigation ##

Suchnavigation ist auf eine Ergebnisseite, häufig die Seite oder oben auf einer Seite am üblich. In Azure-Suche bietet facettierten Navigation Selbststudium absolviert Suche basierend auf vordefinierten Filter. Details finden Sie unter [facettierten Navigation in Azure-Suche](search-faceted-navigation.md) .

## <a name="filters-at-the-page-level"></a>Filter auf Seitenebene ##

Wenn Ihre Lösungsentwurf enthalten dedizierten Suchseiten für bestimmte Arten von Inhalten (beispielsweise eine online-Einzelhandel Anwendung mit Abteilungen oben auf der Seite aufgelistet), können Sie einen Filterausdruck entlang eines **OnClick** -Ereignisses zum Öffnen einer Seite in einem prefiltered Zustand einfügen. 

Sie können einen Filter mit oder ohne einen Suchausdruck senden. Beispielsweise wird die folgende Anforderung filtern, auf Marke Namen zurückgeben nur die Dokumente, die es entsprechen.

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

Weitere Informationen über [Dokumente durchsuchen (Azure suchen-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) finden `$filter` Ausdrücke.

## <a name="see-also"></a>Siehe auch ##

- [Suchen von Azure Service REST-API](http://msdn.microsoft.com/library/azure/dn798935.aspx)
- [Index-Vorgänge](http://msdn.microsoft.com/library/azure/dn798918.aspx)
- [Dokument-Operationen](http://msdn.microsoft.com/library/azure/dn800962.aspx)
- [Video und Lernprogramme zur Azure-Suche](search-video-demo-tutorial-list.md)
- [Facettierten Navigation in Azure suchen](search-faceted-navigation.md)


<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
