<properties 
    pageTitle="So implementieren facettierten Navigation in Azure suchen | Microsoft Azure | Navigationsbereich Kategorien suchen" 
    description="Hinzufügen von facettierten Navigation auf Anwendungen, die in einen Cloud gehosteten Suchdienst auf Microsoft Azure Azure-Suche zu integrieren." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>So implementieren facettierten Navigation in Azure-Suche

Facettierten Navigation ist Filtermechanismen, das Selbststudium absolviert Drilldown-Navigation in Clientanwendungen Suchen bereitstellt. Während der Begriff 'facettierten Navigation' möglicherweise nicht vertraut sind, ist es fast eine vorausgesetzt, dass Sie vor verwendet haben. Wie im folgenden Beispiel wird, ist facettierten Navigationsbereich nicht mehr als die Kategorien verwendet, um die Ergebnisse zu filtern.

## <a name="what-it-looks-like"></a>So sieht es aus

 ![][1]
  
Aspekte helfen Ihnen die gewünschten Informationen finden, sind dabei sicherzustellen, dass Sie wird nicht NULL Ergebnisse zu erhalten. Als Entwickler lassen Sie mit Aspekte besonders hilfreich Suchkriterien für die Navigation in Ihrer Suche trennen verfügbar zu machen. In Clientanwendungen online-Einzelhandel ist facettierten Navigation häufig über Marken, Abteilungen (Kinder Schuhe), Schriftgrad, Preis, Beliebtheit und Bewertungen integriert. 

Implementieren facettierten Navigation unterscheidet sich über Suche Technologien und kann sehr komplex sein. In Azure Suche facettierten Navigation zum Zeitpunkt der Abfrage basiert Attributen Felder in Ihrem Schema zuvor angegeben haben. In den Abfragen, die Ihrer Anwendung erstellt wird, muss eine Abfrage *Vertriebsstrategie Abfrageparameter* senden, um verfügbare Vertriebsstrategie Filterwerte für dieses Dokument Resultset zu erhalten. Das Ergebnis Dokument tatsächlich kürzen festgelegt ist, muss die Anwendung Anwenden einer `$filter` Ausdruck.

Im Bereich der Anwendungsentwicklung bildet Schreiben von Code, Abfragen erstellt wird, den größten Teil der Arbeit aus. Viele der Anwendung Verhaltensweisen, die von der Navigation mit facettierten sinnvoll ist wird durch den Dienst, einschließlich der integrierten Unterstützung für Bereiche einrichten und zählt Vertriebsstrategie Ergebnisse erzielen Sie erste bereitgestellt. Der Dienst enthält auch sinnvolle Standardeinstellungen, mit denen Sie schwerfällig Navigationsstrukturen zu vermeiden. 

Dieser Artikel enthält die folgenden Abschnitte:

- [So erstellen Sie](#howtobuildit)
- [Erstellen des Präsentation Layers](#presentationlayer)
- [Erstellen des Indexes](#buildindex)
- [Prüfen der Qualität der Daten](#checkdata)
- [Erstellen der Abfrage](#buildquery)
- [Tipps zur Steuerung der facettierten navigation](#tips)
- [Facettierten Navigation basierend auf den Bereichswerte](#rangefacets)
- [Facettierten Navigation basierend auf GeoPoints](#geofacets)
- [Probieren Sie es aus](#tryitout)

##<a name="why-use-it"></a>Warum verwenden diese
Die effektivsten Suche Applikationen haben mehrere Interaktion Modelle als ein Suchfeld. Facettierten Navigation ist eine einen alternativen Einstiegspunkt zum Suchen, bietet eine geeignete Alternative zu komplexer Suchausdrücke von hand eingeben.

##<a name="know-the-fundamentals"></a>Kennen Sie die Grundlagen

Wenn Sie neu bei der Entwicklung suchen sind, ist die beste Methode zum facettierten Navigation vorstellen, dass mögliche Werte für die Suche Selbststudium absolviert angezeigt. Es ist eine Art von Drilldown-Suchfunktion, basierend auf vordefinierten Filter für schnell Eingrenzung Suchergebnisse durch Point-und-Click Aktionen verwendet. 

**Interaktionsmodell**

Die Suchfunktion für facettierten Navigation ist iterative, uns zunächst verstehen dieses als eine Abfolge von Abfragen, die als Antwort auf Aktionen des Benutzers zu öffnen.

Ausgangspunkt ist eine Anwendungsseite, die in der Regel auf der Peripherie platziert facettierten Navigation bietet. Facettierten Navigation ist oft eine Struktur mit Kontrollkästchen für jeden Wert oder Text geklickt werden kann. 

1.  Eine Abfrage zu Azure gesendet gibt die facettierten Navigationsstruktur über einen oder mehrere Vertriebsstrategie Abfrageparameter an. Beispielsweise die Abfrage enthalten möglicherweise `facet=Rating`, vielleicht mit einer `:values` oder `:sort` Option, um die Präsentation zu optimieren.
2.  Die Präsentationsebene gibt eine Suchseite, die facettierten Navigation, verwenden die Aspekte, die bei der Anforderung angegebenen bietet wieder.
3.  Ausgehend von einer facettierten Navigationsstruktur, die Bewertung enthält, der Benutzer klickt "4" um anzugeben, dass nur Produkte mit einer Bewertung von 4 oder höher angezeigt werden sollen. 
4.  In der Antwort sendet die Anwendung enthält eine Abfrage mit`$filter=Rating ge 4` 
5.  Aktualisiert die Seite, mit einer reduzierten Resultset, enthält nur die Elemente, die die neuen Kriterien erfüllen, der Layer Präsentation (in diesem Fall bewertet Produkte 4 und höher).

Eine Facette ist ein Abfrageparameter, aber nicht mit der Eingabe der Abfrage Begriff. Es wird nie als Kriterien für die Auswahl in einer Abfrage verwendet werden. In diesem Fall Vertriebsstrategie Abfrageparameter als Eingaben für die Navigationsstruktur, die in der Antwort zurückkommt vorstellen. Für jeden Aspekt Abfrageparameter, die Sie bereitstellen, wird Azure Suche ausgewertet werden, wie viele Dokumente in die teilweise Ergebnisse für jeden Wert Vertriebsstrategie befinden.

Hinweis Die `$filter` in Schritt 4. Dies ist ein wichtiger Aspekt der facettierten Navigation. Obwohl Aspekte und Filter in der-API unabhängig sind, benötigen Sie sowohl die Oberfläche vorführen, die Sie ausgefüllt werden sollen. 

**Entwurfmustern**

Die Vorgehensweise besteht im Code der Anwendung Vertriebsstrategie Abfrageparameter zurückzugebenden facettierten Navigationsstruktur zusammen mit Vertriebsstrategie Ergebnisse sowie eine $filter-Ausdruck, der das Ereignis auf verarbeitet verwenden. Stellen Sie vor der `$filter` Ausdruck wie der Code hinter der tatsächlichen Verkürzen der Suchergebnisse auf die Präsentationsebene zurückgegeben. Angegebenen eine Facette Farben, auf die Farbe Rot implementiert ist, bis eine `$filter` Ausdruck, der nur die Elemente markiert, die eine rote Farbe aufweisen. 

**Abfrage-Grundlagen in Azure suchen**

In Azure suchen wird eine Anforderung durch eine oder mehrere Abfrageparameter angegeben (eine Beschreibung der einzelnen finden Sie unter [Dokumente durchsuchen](http://msdn.microsoft.com/library/azure/dn798927.aspx) ). Keiner der Abfrageparameter ist erforderlich, aber Sie müssen mindestens eine damit eine Abfrage gültig ist.

Genauigkeit, als die Möglichkeit zum Filtern von irrelevante Treffer, in der Regel verstanden wird durch eine oder beide der folgenden Ausdrücke erreicht:

- **Suche =**<br/>
Der Wert für diesen Parameter bildet den Suchausdruck. Es möglicherweise kaufmännischen Text oder einer komplexen Suchausdruck, die mehrere Ausdrücke und Operatoren enthält. Auf dem Server wird eine Suchausdruck für Voll-Textsuche Abfragen durchsuchbare Felder im Index für den Abgleich Ausdrücke, Zurückgeben von Ergebnissen in der Rangfolge verwendet. Wenn Sie festlegen `search` auf null, Abfrage Ausführung über den gesamten Index ist (d. h., `search=*`). In diesem Fall, andere Elemente der Abfrage, wie eine `$filter` oder bewerten Profil, werden die primären Faktoren, welche Dokumente beeinflussen zurückgegeben werden `($filter`) und in welcher Reihenfolge (`scoringProfile` oder `$orderb`y).

- **$filter =**<br/>
Ein Filter ist eine leistungsfähige Methode zum Begrenzen der Größe der Suchergebnisse basierend auf den Werten der Attribute bestimmtes Dokument an. A `$filter` zuerst ausgewertet, gefolgt von Faceting Logik, die die verfügbaren Werte und der entsprechenden Anzahl für jeden Wert generiert wird

Komplexe Suchausdrücke werden die Leistung der Abfrage verkleinern. Soweit möglich, wohlformulierte Filterausdrücke zur Erhöhung der Genauigkeit und verbessern die Leistung von Abfragen zu nutzen.

Um besser zu verstehen, wie ein Filter präzisere addiert, vergleichen Sie einer komplexen Suchausdruck hinzu, die einen Filterausdruck umfasst:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Obwohl beide Abfragen gültig sind, ist das zweite überlegen, wenn für nicht aus mit Parken in Seattle gefunden haben. Die erste Abfrage basiert auf diese bestimmte Wörter, die angegeben ist oder nicht erwähnt Zeichenfolge Felder wie Name, Beschreibung und einem der anderen Felder, die durchsucht werden Daten enthalten. Die zweite Abfrage für die genaue Übereinstimmung auf strukturierte Daten sucht und wahrscheinlich sehr viel mehr genau ist.

In Clientanwendungen, die facettierten Navigation enthalten, sollten Sie um sicherzustellen, dass jede Benutzeraktion über eine facettierten Navigationsstruktur begleitet ist durch eine Einschränken der Suchergebnisse, bis eines Filterausdrucks erreicht.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>So erstellen Sie

Facettierten Navigation in Azure Suche wird in Ihrer Anwendungscode implementiert, die die Anforderung erstellt, aber auf vordefinierte Elemente in Ihrem Schema basiert.

Vordefinierte in Ihrer Suche Index ist die `Facetable [true|false]` Indexattribut, aktivieren oder deaktivieren ihre Verwendung in einem facettierten Navigationsstruktur ausgewählten Felder festlegen. Ohne `"Facetable" = true`, ein Feld kann nicht in der Navigation Vertriebsstrategie verwendet werden.

Zum Zeitpunkt der Abfrage erstellt der Anwendungscode enthält Anforderung eine `facet=[string]`, einem Anforderungsparameter, Vertriebsstrategie, indem Sie das Feld bereitstellt. Eine Abfrage haben mehrere Aspekte, wie `&facet=color&facet=category&facet=rating`, können Sie jeweils durch ein kaufmännisches und-Zeichen (&) getrennt.

Anwendungscode muss auch Erstellen einer `$filter` Ausdruck, der die Ereignisse klicken Sie auf facettierten Navigationsbereich zu behandeln. A `$filter` reduziert die Suchergebnissen mit dem Vertriebsstrategie Wert als Filterkriterien.

Azure Suche gibt die Suchergebnissen pro die vom Benutzer, zusammen mit Updates zur facettierten Navigationsstruktur eingegebenen Begriffe. In Azure Suche facettierten Navigation ist eine Ebene Konstruktion, bei Vertriebsstrategie Werten und zählt, wie viele Ergebnisse für jedes ausgeschlossene gefunden werden.

Die Präsentationsebene im Code enthält die Benutzerfunktionalität. Es sollte die Bestandteile der facettierten Navigation, wie die Beschriftung, Werte, Kontrollkästchen und die Anzahl der Liste. Die REST-API von Azure Suche ist plattformunabhängig, verwenden Sie also versetzt Sprache und Plattform werden sollen. Wichtige Hinweis besteht darin, die Elemente der Benutzeroberfläche enthalten, die inkrementell aktualisieren, mit der aktualisierten Benutzeroberflächenzustand unterstützen, wie jede zusätzliche Facette ausgewählt ist. 

In den folgenden Abschnitten werden wir Detail So erstellen Sie jeden Teil dauern, beginnend mit der Präsentationsebene.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Erstellen des Präsentation Layers

Arbeit wieder aus der Präsentationsebene hilft Ihnen ein Anforderungen, die andernfalls verpasst werden möglicherweise und verstanden für eine Steigerung die Funktionen für die Suchfunktion wichtig sind.

Im Hinblick auf facettierten Navigation Ihrer Seite Web oder einer Anwendung zeigt die facettierten Navigationsstruktur, erkennt Benutzereingabe auf der Seite, und fügt die geänderten Elemente. 

Für Webanwendungen AJAX häufig in der Präsentationsebene verwendet, da es Ihnen ermöglicht, ändert aktualisieren. Sie können auch ASP.NET-MVC oder eine andere Visualisierung Plattform, die mit einer Azure Suchdienst über HTTP herstellen kann. Beispiel für Anwendung handelt, die in diesem Artikel – **AdventureWorks Katalog** – geschieht mit einer ASP.NET-MVC-Anwendung werden.

Im folgenden Beispiel, die Datei **index.cshtml** von der Anwendung, die Stichprobe entnommen erstellt eine dynamische HTML-Struktur für die Anzeige von facettierten Navigation in der Suchergebnisseite an. In der Stichprobe facettierten Navigation ist in der Suchergebnisseite integriert und wird nach der Benutzer einen Suchbegriff übermittelt hat.

Beachten Sie, dass jede Facette eine Bezeichnung (Farben, Kategorien, Preise), einer Bindung an ein facettierten Feld (Farbe, Kategoriename, ListPrice) und einer hat `.count` Parameter, verwendet, um die Anzahl der Elemente, die für dieses Ergebnis Vertriebsstrategie gefunden zurückzugeben.

  ![][2]
 

> [AZURE.TIP] Beim Entwerfen der Suchergebnisseite, denken Sie daran, die ein Verfahren zum Löschen des Aspekte hinzufügen. Wenn Sie die Kontrollkästchen verwenden, können Benutzer einfach so deaktivieren Sie die Filter intuit. Für andere Layouts benötigen Sie möglicherweise ein Breadcrumb Muster oder ein anderer kreative Ansatz. In diesem Beispiel AdventureWorks Katalog, können Sie beispielsweise den Titel, AdventureWorks-Katalog auf die Suchseite zurücksetzen klicken.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Erstellen des Indexes

Faceting auf Grundlage von Feld im Index, über dieses Indexattribut aktiviert ist: `"Facetable": true`.  
Werden alle Feldtypen, für die facettierten Navigationsbereich eventuell verwendet werden könnte `Facetable` standardmäßig. Solche Feldtypen gehören `Edm.String`, `Edm.DateTimeOffset`, und alle numerischen Feldtypen (alle Feldtypen sind im Wesentlichen Facetable außer `Edm.GeographyPoint`, die nicht in facettierten Navigation verwendet werden). 

Beim Erstellen eines Indexes ist eine bewährte Methode für die Navigation facettierten explizit Faceting für Felder deaktivieren, die als eine Facette nie verwendet werden soll.  Felder für einzelne Werte, wie etwa einen Namen ID oder Produkt Zeichenfolge sollte vor allem auf festgelegt werden `"Facetable": false` zu deren Verwendung unbeabsichtigte (und nicht wirksam) in eine facettierten Navigation zu verhindern.

Es folgt das Schema für die AdventureWorks Katalog Beispiel-app (einige Attribute Gesamtgröße reduzieren gekürzt):

 ![][3]
 
Beachten Sie, dass `Facetable` deaktiviert ist, für die Felder für die Zeichenfolge, die als Aspekte, wie z. B. eine ID oder einen Namen verwendet werden. Umwandlung Faceting deaktivieren, wo Sie es nicht benötigen hilft, die Größe des Indexes gering zu halten, und im Allgemeinen wird die Leistung verbessert.

> [AZURE.TIP] Als bewährte Methode aufnehmen möchten Sie sämtlicher Indizieren von Attributen für jedes Feld aus. Obwohl `Facetable` ist standardmäßig für fast alle Felder, die absichtlich festlegen jedes Attribut helfen Ihnen, bis die Auswirkungen der einzelnen Schema Entscheidung vorstellen. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Prüfen der Qualität der Daten 

Wenn Sie alle Daten reduzierte Anwendung entwickeln, ist Vorbereiten der Daten häufig eine größere Teile des Projekts. Suchen von Applications sind keine Ausnahmen. Die Qualität Ihrer Daten verfügt über eine direkte Einfluss darauf, ob die facettierten Navigationsstruktur materialisiert, wie er festlegen sowie deren Effektivität erwartet helfen Ihnen die Filter zu erstellen, die das Ergebnis zu verringern.

In Azure suchen wird die Suche Trennen von Dokumenten geformt, die einen Index zu füllen. Dokumente geben Sie die Werte, die verwendet werden, um Aspekte zu berechnen. Wenn Sie nach Marke oder Kurs Vertriebsstrategie möchten, sollten jedes Dokument Werte enthalten, für *BrandName* und *ProductPrice* , die gültig, konsistent und als eine Filteroption aus produktiv sind.

Einige Erinnerungen was für bereinigen werden nachfolgend aufgeführt:

- Fragen Sie für jedes Feld die gewünschten Vertriebsstrategie durch sich, ob sie Werte enthält, die in Selbststudium absolviert Suche als Filter geeignet sind. Die Werte sollten kurzen, aussagekräftigen und ausreichend besondere klar zwischen verschiedenen Optionen die Möglichkeit bieten.
- Rechtschreib- oder nahezu übereinstimmende Werte. Wenn Vertriebsstrategie Farbe und die Feldwerte enthalten, Orange und Ornage (falsch geschrieben), eine Facette basierend auf dem Feld Farbe einrichten beide auswählen möchten.
- Gemischte Groß-/Kleinschreibung Text kann auch Verhinderung facettierten Navigationsbereich mit Orange und Orange als zwei verschiedenen Werte angezeigte größerer. 
- Eine separate Facette können für jeden einzelnen und Pluralformen Versionen des gleichen Wert führen.

Wie Sie sich vorstellen können, ist Diligence Vorbereiten der Daten ein wichtiger Aspekt des effektiven facettierten Navigation aus.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Erstellen der Abfrage

Code, den Sie zum Erstellen von Abfragen schreiben sollten alle Teile einer gültigen Abfrage, einschließlich Suchausdrücke, Aspekte, filtern, bewerten Profile – nichts verwendet, um eine Anforderung Formulierung angeben. In diesem Abschnitt werden vorgestellt, in dem Aspekte in einer Abfrage anpassen, und wie Filter mit Aspekten zum Übermitteln einer reduzierten Resultset verwendet werden.

Ein Beispiel ist häufig ein guter beginnen. Im folgenden Beispiel, aus der Datei **CatalogSearch.cs** erstellt eine Anforderung, die Vertriebsstrategie Navigation basierend auf Farbe, Category und Price erstellt wird. 

Beachten Sie, dass Aspekte in diesem Beispiel Anwendung ganzzahligen sind. Die Suchfunktion im Katalog AdventureWorks dient um facettierten Navigation und Filter. Dies fällt in herausragender Platzierung des facettierten Navigation auf der Seite. Die Anwendung Stichprobe enthält URI-Parameter für Aspekte (Farbe, Kategorie, Preise) als Eigenschaften für die Suche Methode (wie in diesem Beispiel erstellt).

  ![][4]
 
Ein Abfrageparameter Vertriebsstrategie in ein Feld festgelegt ist und in Abhängigkeit zwischen dem Datentyp kann werden weitere parametrisierte durch durch Trennzeichen getrennte Liste enthält `count:<integer>`, `sort:<>`, `intervals:<integer>`, und `values:<list>`. Beim Einrichten von Bereichen, wird eine Werteliste für numerische Daten unterstützt. Verwendungsdetails finden Sie unter [Dokumente durchsuchen (Azure Suche API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Zusammen mit Aspekte sollte die Anfrage nach Ihrer Anwendung formuliert auch Filter zu begrenzen die Menge der Kandidat Dokumente basierend auf der Auswahl Wert Vertriebsstrategie erstellen. Für einen Speicher bewältigen kann, bietet facettierten Navigation Hinweise bereitgestellt werden, Fragen wie "welche Farben, Hersteller und Typen von Bikes stehen", während des Filterns Fragen wie "welche genauen Fahrräder rot angezeigt werden, Mountainbikes, diese Preis Bereich" Antworten.

Wenn ein Benutzer "Rot" klickt, um anzugeben, dass nur die rote Produkte angezeigt werden sollen, zählen die nächste Abfrage die Anwendung sendet `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Bewährte Methoden für die facettierten navigation

In der folgenden Liste sind einige bewährte Methoden zusammengefasst.

- **Genauigkeit**<br/>
Verwenden Sie Filter ein. Wenn Sie sich auf nur die alleine, Wortstamm verursachen ein Dokuments, der zurückgegeben wird, die keine den präzise Vertriebsstrategie Wert in einem der Felder besitzt Suchausdrücke verlassen. 

- **Zielfelder**<br/>
Facettierten Drilldown, Sie normalerweise nur Dokumente aufnehmen möchten Sie, die den Wert Vertriebsstrategie nicht an einer beliebigen Stelle in einem bestimmten Feld (facettierten), über alle durchsuchbare Felder enthalten. Hinzufügen eines Filters verstärkt das Zielfeld, indem Sie den Dienst nur im Feld facettierten nach einem übereinstimmenden Wert suchen.

- **Index Effizienz**<br/>
Wenn die Anwendung ausschließlich facettierten Navigation verwendet (d. h., keine Suchfeld), können Sie das Feld als markieren `searchable=false`, `facetable=true` um eine kompaktere Index zu erstellen. Darüber hinaus erfolgt die Indizierung nur auf die gesamte Vertriebsstrategie Werte, aber keine Word-Seitenumbruch oder die Komponenten eines Werts mit mehreren Word indizieren.

- **Leistung**<br/>
Filter Grenzen Sie die Menge der Kandidat Dokumente für die Suche und aus Rangfolge ausgeschlossen. Wenn Sie eine große Gruppe von Dokumenten verfügen, erhalten verwenden eine sehr selektiven Vertriebsstrategie Drillup nach unten häufig erheblich eine bessere Leistung Sie.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Tipps zur Steuerung der facettierten navigation

Es folgt ein Tipp Blatt mit Anleitungen auf bestimmte Probleme.

**Hinzufügen von Beschriftungen für jedes Feld Vertriebsstrategie Navigationsbereich**

Etiketten sind in der Regel in den HTML-Formular (in diesem Beispiel**index.cshtml** ) definiert. Es gibt keine API in Azure Suchen nach Vertriebsstrategie Navigation Etiketten oder alle anderen Arten von Metadaten aus.

**Definieren Sie, welche Felder als Vertriebsstrategie verwendet werden kann**

Denken Sie daran, dass das Schema des Indexes bestimmt, welche Felder als eine Facette Verwendung verfügbar sind. Vorausgesetzt, dass ein Feld Facetable ist, gibt die Abfrage, die zum Vertriebsstrategie, indem Sie Felder. Das Feld, an dem Sie Faceting sind, enthält die Werte, die unterhalb der Bezeichnung angezeigt werden. 

Die Werte, die unter jedem Etikett angezeigt werden aus dem Index abgerufen. Zum Beispiel ist das Feld "Vertriebsstrategie" *Farbe*, sein die Werte für die zusätzliche Filter verfügbar die Werte für dieses Feld (Rot, Schwarz usw.).

Sie können für numerische und DateTime-Werte nur explizit Werte festlegen, klicken Sie auf das Feld "Facette" (z. B. `facet=Rating,values:1|2|3|4|5`). Für die folgenden Feldtypen zu vereinfachen, die Aufteilung Vertriebsstrategie Ergebnisse in zusammenhängende Bereiche (entweder Bereiche basierend auf numerische Werte oder Zeiträumen) ist eine Werteliste zulässig. 

**Kürzen Vertriebsstrategie Ergebnisse**

Vertriebsstrategie Ergebnisse sind Dokumente, die in den Suchergebnissen aus, die einen Ausdruck Vertriebsstrategie entsprechen gefunden. Im folgenden Beispiel haben in den Suchergebnissen für *Cloud computing*, 254 Elemente auch *internen Spezifikation* als einen Inhaltstyp. Elemente sind nicht unbedingt gegenseitig. Wenn ein Element den Suchkriterien beide Filter entspricht, wird es in den einzelnen gezählt. Dies ist möglich, wenn Faceting auf `Collection(Edm.String)` Felder, die häufig verwendet werden, um das Dokument zu kategorisieren implementieren.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Im Allgemeinen, wenn Sie feststellen, dass Vertriebsstrategie Ergebnisse werden dauerhaft zu groß, wir empfehlen, die Sie weitere Filter hinzufügen, wie in den vorherigen Abschnitten, um weitere Optionen zum Eingrenzen der Suche geben Sie Ihre Anwendungsbenutzer erläutert.

**Elemente im Navigationsbereich Vertriebsstrategie einschränken**

Für jedes facettierten Feld in der Navigationsstruktur ist es standardmäßig maximal 10 Werte. Diese Standardeinstellung ist sinnvoll für Navigationsstrukturen, da es die Werteliste auf eine verwaltbare Größe behält. Sie können die Standardeinstellung außer Kraft setzen, durch Zuweisen eines Werts zu zählen.

- `&facet=city,count:5`Gibt an, dass nur die ersten 5 Orte in der oberen eingestuft Ergebnisse gefunden als Ergebnis Vertriebsstrategie zurückgegeben werden. Erhalten einen Suchbegriff "Airport" und 32 entspricht, wenn die Abfrage gibt an, `&facet=city,count:5`, werden nur die ersten fünf eindeutigen Orte mit Dokumenten, die am häufigsten in den Suchergebnissen Vertriebsstrategie Ergebnisse enthalten.

Beachten Sie den Unterschied zwischen Vertriebsstrategie Ergebnisse und Suchergebnissen. Suchergebnisse sind alle Dokumente, die die Abfrage entsprechen. Vertriebsstrategie Ergebnisse werden die Übereinstimmungen bereits für jeden Wert Vertriebsstrategie. Im Beispiel werden die Suchergebnisse Stadtnamen enthalten, die nicht in der Liste von Vertriebsstrategie Klassifikation (in diesem Beispiel 5) sind. Ergebnisse, die durch die facettierten Navigation herausgefiltert werden für den Benutzer sichtbar, wenn er Aspekte löscht, oder andere Aspekte neben Ort wählt. 

> [AZURE.NOTE] Besprochen `count` wenn vorhanden ist kann mehrere Typen verwirrend sein. Die folgende Tabelle bietet einen kurzen Überblick darüber, wie der Begriff in Azure Suche API, Beispiel-Code und Dokumentation verwendet wird. 

- `@colorFacet.count`<br/>
Präsentation Code sollte Count-Parameter auf die Vertriebsstrategie, verwendet, um die Anzahl der Vertriebsstrategie Ergebnisse anzeigen angezeigt werden. Zählen in Suchergebnissen Vertriebsstrategie zeigt an, dass die Anzahl der Dokumente, die auf die Vertriebsstrategie Ausdruck oder einen Zellbereich entsprechen.

- `&facet=City,count:12`<br/>
In einer Abfrage Vertriebsstrategie können Sie die Anzahl auf einen Wert festlegen.  Die Standardeinstellung ist 10, aber Sie können festlegen, oben oder unten. Festlegen von `count:12` Ruft im oberen Bereich 12 in Vertriebsstrategie Ergebnisse nach Dokument zählen entspricht.

- "`@odata.count`"<br/>
In der Antwort auf die Abfrage gibt dieser Wert die Anzahl der übereinstimmenden Elemente in den Suchergebnissen angezeigt. Durchschnitt, es ist größer als die Summe aller Vertriebsstrategie Ergebnisse kombiniert, aufgrund von dem Vorhandensein von Elementen, die den Suchbegriff entsprechen aber keine Vertriebsstrategie Wert entspricht.


**Ebenen in facettierten navigation** 

Wie erwähnt, gibt es keine direkte Unterstützung für das Verschachteln von Aspekte in einer Hierarchie ein. Nutzen Sie im Feld unterstützt facettierten Navigation nur eine Ebene Filter aus. Problemumgehung führen Sie allerdings vorhanden sein. Sie können eine Vertriebsstrategie hierarchische Struktur in Codieren einer `Collection(Edm.String)` zeigen Sie mit einem Eintrag pro Hierarchie. Implementieren diese problemumgehung ist nicht Gegenstand dieses Artikels, aber Sie erhalten Informationen über Websitesammlungen in [OData anhand eines Beispiels](http://msdn.microsoft.com/library/ff478141.aspx)aus. 

**Überprüfen Sie die Felder**

Wenn Sie die Liste der Aspekte dynamisch basierend auf nicht vertrauenswürdigen Benutzereingabe erstellen, sollten Sie entweder überprüfen, ob die Namen der Felder facettierten gültig sind oder die Namen escape-beim Erstellen von URLs verwenden entweder `Uri.EscapeDataString()` in .NET oder die Entsprechung in Ihre Plattform Wahl.

**Zählt die Vertriebsstrategie Ergebnisse**

Beim Hinzufügen eines Filters zu einer Abfrage facettierten möglicherweise möchten Sie die Vertriebsstrategie-Anweisung beibehalten (z. B. `facet=Rating&$filter=Rating ge 4`). Technisch, Vertriebsstrategie = Bewertung ist nicht erforderlich, aber es planmäßigen gibt die Anzahl der Vertriebsstrategie Werte für Bewertungen, 4 und höher. Beispielsweise werden, wenn ein Benutzer klickt "4", und die Abfrage einen Filter für größer oder gleich "4 enthält", zählt zurückgegeben für jede Bewertung, die 4 und höher.  

**Sharding Auswirkungen auf Vertriebsstrategie zählt**

Unter bestimmten Umständen können es passieren, dass Vertriebsstrategie zählt die Ergebnismengen stimmen nicht überein (siehe [facettierten Navigation in Azure-Suche (Forum Post)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Vertriebsstrategie zählt können falsche wegen der Architektur Sharding sein. Jeder Suchindex enthält mehrere mehrere Shards hinweg, und können Sie jeweils die obersten N Aspekte meldet, indem Dokumentanzahl – dort werden dann in ein einzelnes Ergebnis kombiniert werden. Wenn einige mehrere Shards hinweg viele übereinstimmende Werte aufweisen haben, während andere kleiner haben, kann es passieren, dass einige Vertriebsstrategie Werte fehlen oder unter-in den Ergebnissen berücksichtigt.

Obwohl dieses Verhalten zu einem beliebigen Zeitpunkt, ändern kann, wenn dieses Verhalten heute auftreten, arbeiten zu können versehen künstlich überhöhte die Anzahl:<number> zu einer großen Anzahl zum Erzwingen der vollständigen Berichte aus der einzelnen Shard. Wenn der Wert von Count: ist größer als oder gleich der Anzahl der eindeutigen Werte in das Feld, werden die genauen Ergebnisse garantiert. Jedoch wenn Dokument zählt wirklich hohe Leistung beeinträchtigt vorhanden ist sind, verwendet, damit diese Option überlegt.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Vertriebsstrategie Navigation auf der Grundlage einer Bereichswerte

Faceting über Bereiche ist eine allgemeine Suche Anwendung Anforderung an. Bereiche werden für numerische Daten und DateTime-Werte unterstützt. Weitere Informationen zu jeder Ansatz in [Dokumente durchsuchen (Azure Suche API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure Suche vereinfacht Bereich Bauweise durch zwei Vorgehensweisen zum Berechnen eines Bereichs. Für beide Methoden erstellt Azure Suche entsprechenden Bereiche, die die von Ihnen bereitgestellten Eingaben angegeben. Z. B., wenn Sie den Bereichswerte von 10 angeben | 20 | 30, wird es automatisch Zellbereiche 0-10, 10 bis 20, 20-30 erstellen. Die Anwendung Beispiel entfernt alle Intervalle, die leer sind. 

**Methode 1: Verwenden Sie den Parameter Intervall**<br/>
Wenn Kurs Aspekte in $10 Schritten festlegen möchten, würden Sie Folgendes angeben:`&facet=price,interval:10`


**Verfahren 2: Verwenden einer Werteliste**<br/>
Numerische Daten können Sie eine Werteliste.  Berücksichtigen Sie Vertriebsstrategie Bereich für ListPrice, gerendert wie folgt aus:

  ![][5]

Der Bereich wird in der **CatalogSearch.cs** -Datei mit einer Werteliste angegeben:

    facet=listPrice,values:10|25|100|500|1000|2500

Jeder Bereich basiert, verwenden 0 als Ausgangspunkt, einen Wert aus der Liste als Endpunkt, und klicken Sie dann des vorherigen Bereichs diskrete Intervallen erstellen zugeschnitten. Azure Suche wird als Teil des facettierten Navigation ausführen. Sie müssen keinen Code für jedes Intervall Strukturierung schreiben.

### <a name="build-a-filter-for-facet-ranges"></a>Erstellen Sie einen Filter für Bereiche Vertriebsstrategie ###

Um Dokumente basierend auf einem vom Benutzer ausgewählten Bereich zu filtern, können Sie die `"ge"` und `"lt"` Filtern Operatoren in einem Ausdruck von zwei Teilen, die die Endpunkte des Bereichs definiert. Angenommen, wenn der Benutzer den Bereich 10-25, wählt der Filter wäre `$filter=listPrice ge 10 and listPrice lt 25`.

In diesem Beispiel mithilfe der Filterausdruck **Servicelevel** und **PriceTo** Parameter die Endpunkte festgelegt. Die Methode **BuildFilter** in **CatalogSearch.cs** enthält den Filter-Ausdruck, der Ihnen ermöglicht, dass die Dokumente in einem Bereich an.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Gefilterte Navigation basierend auf GeoPoints

Es gemeinsamer zu finden Sie unter Filter, die Ihnen helfen, die Sie einer Store, Restaurant oder basierend auf deren Nähe zu Ihren aktuellen Standort Ziel auswählen. Während Sie diese Art von Filter facettierten Navigation aussehen kann, ist es tatsächlich nur einen Filter aus. Wir Erwähnen sie hier für diejenigen, die speziell für Implementierung Ratschläge für die betreffenden Entwurfsproblem gefunden werden.

Es gibt zwei Geodaten-Funktionen in Azure suchen, **geo.distance** und **geo.intersects**aus.

- Die **geo.distance** -Funktion gibt den Abstand in Kilometer zwischen zwei Punkten, eine ein Feld und eine Konstante wird übergebene als Teil der Filters. 

- True, wenn ein bestimmter Punkt innerhalb eines bestimmten Polygons, ist wobei der Punkt eines Felds und Polygons als eine Konstante Liste als Teil der Filters übergebenen Koordinaten angegeben, gibt die Funktion **geo.intersects** zurück.

Filter-Beispiele finden Sie in der [OData-Ausdruckssyntax (Azure-Suche)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Probieren Sie es aus

Azure Suche Adventure Works-Demo auf Codeplex enthält die Beispiele in diesem Artikel verwiesen wird. Beim Arbeiten mit Suchergebnissen, schauen Sie sich die URL für die Änderungen in Erstellen der Abfrage. Diese Anwendung geschieht beim Auswählen der einzelnen Aspekte der URI angefügt.

1.  Konfigurieren der Stichprobe Anwendungs das Dienst-URL und api-Taste verwenden. 

    Beachten Sie das Schema, das in der Datei Program.cs des Projekts CatalogIndexer definiert ist. Es gibt Facetable Felder für Farbe, ListPrice, Schriftgrad, Schriftbreite, Kategoriename und ModelName an.  Nur ein Paar der diese (Farbe, ListPrice, Kategoriename) sind eigentlich facettierten Navigationsbereich implementiert.

3.  Führen Sie die Anwendung. 

    Zuerst wird nur das Suchfeld angezeigt. Sie können auf die Schaltfläche Suchen klicken sofort, um alle Ergebnisse zu erhalten, oder geben Sie einen Suchbegriff.

    ![][7]
 
4.  Geben Sie einen Suchbegriff wie bewältigen kann, und klicken Sie auf **Suchen**. Die Abfrage wird schnell ausgeführt.
 
    Eine facettierten Navigationsstruktur wird auch mit den Suchergebnissen zurückgegeben.  In der URL sind für Farben, Kategorien und Preise Aspekte null. In der Suchergebnisseite enthält die facettierten Navigationsstruktur zählt jedes Vertriebsstrategie Ergebnis.

     ![][8]
 
5.  Klicken Sie auf eine Farbe, Category und Price Bereich. Aspekte sind bei der anfänglichen einer erweiterten Suche null, aber wie sie auf Werte übernehmen, werden die Suchergebnisse von Elementen, die nicht mehr entsprechen abgeschnitten. Beachten Sie, dass der URI von der Änderungen an Ihrer Abfrage wählt.

    ![][9]
 
6.  Klicken Sie zum Löschen der facettierten Abfrage, damit Sie versuchen können, andere Abfrage Verhaltensweisen **AdventureWorks Katalog** am oberen Rand der Seite auf.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Als Nächstes

Klicken Sie zum Testen Ihr Wissen, können Sie ein Feld Vertriebsstrategie für *%{modelname/*hinzufügen. Der Index ist bereits für diese Vertriebsstrategie, festgelegt, sodass keine Änderungen an den Index erforderlich sind. Müssen Sie jedoch wird den HTML-Code zum ist eine neue Facette für Datenmodelle enthalten, und fügen Sie das Feld "Vertriebsstrategie" an den Konstruktor Abfrage ändern.

Für weiteren Einsichten auf Entwurfsprinzipien für facettierten Navigation empfehlen wir die folgenden Links:

- [Erstellen eines Konzepts für facettierten suchen](http://www.uie.com/articles/faceted_search/)
- [Entwurfmustern: Facettierten Navigation](http://alistapart.com/article/design-patterns-faceted-navigation)

Sie können auch [Azure Suche aneignen](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410)anschauen. Bei 45:25 gibt es eine Demo zum Aspekte implementieren.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 