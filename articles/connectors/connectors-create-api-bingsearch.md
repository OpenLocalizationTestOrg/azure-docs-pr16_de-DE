<properties
    pageTitle="Hinzufügen der Bing-Suchfeld Verbinder Logik apps | Microsoft Azure"
    description="Übersicht über den Verbinder Bing-Suchfeld mit den Parametern REST-API"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-bing-search-connector"></a>Erste Schritte mit der Bing-Suchfeld Verbinder 
Verbinden Sie mit Bing suchen, um Nachrichten, durchsucht Videos und weitere zu suchen. Mit Bing suchen können Sie folgende Aktionen ausführen: 

- Erstellen Sie Ihr Unternehmen Fluss basierend auf den Daten, die Sie bei der Suche zu erhalten. 
- Verwenden von Aktionen zu Bildern suchen, suchen die Informationen und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Sie können beispielsweise für ein Video suchen und verwenden Sie dann Twitter an, die für einen Twitter-feed video Posten.

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Bing-Suchfeld umfasst die folgenden Aktionen aus. Es gibt keine Trigger aus. 

Trigger | Aktionen
--- | ---
Keine | <ul><li>Web durchsuchen</li><li>Suchen von videos</li><li>Suchen von Bildern</li><li>Suchen von Informationen</li><li>Zusammenhang suchen</li><li>Suche Rechtschreibung</li><li>Alle suchen</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten.


## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
Version gilt: 1.0.

### <a name="search-web"></a>Web durchsuchen 
Ruft Websites von einer Bing-Suchfeld ein.  
```GET: /Web```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage| keine|Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage|keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage| keine|Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage| keine|Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|
|webFileType|Zeichenfolge|Nein|Abfrage|keine |Dateityp zum Eingrenzen der Suche (Beispiel: 'Dokument')|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="search-videos"></a>Suchen von videos 
Ruft Videos von einer Bing-Suchfeld ein.  
```GET: /Video```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage| keine|Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage|keine |Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage|keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage|keine |Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage|keine |Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|
|videoFilters|Zeichenfolge|Nein|Abfrage|keine |Suche basierend auf Größe, Aspekt, Farbe, Formatvorlage, Smiley oder einer beliebigen Kombination daraus zu filtern.  Gültige Werte: <ul><li>Dauer: kurz</li><li>Dauer: Mittel</li><li>Dauer: lange</li><li>Seitenverhältnis: Standard</li><li>Aspekt: Breitbild</li><li>Lösung: "Niedrig"</li><li>Lösung: Mittel</li><li>Lösung: hoch</li></ul> <br/><br/>Beispiel: 'Dauer: kurz + Auflösung: hoch'|
|videoSortBy|Zeichenfolge|Nein|Abfrage|keine |Sortierreihenfolge für Ergebnisse. Gültige Werte: <ul><li>Datum</li><li>Relevanz</li></ul> <p>Sortierreihenfolge Datum impliziert absteigender Reihenfolge.</p>|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="search-images"></a>Suchen von Bildern    
Ruft Bilder von einer Bing-Suchfeld ein.  
```GET: /Image```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage|keine |Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage|keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage| keine|Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage|keine |Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|
|imageFilters|Zeichenfolge|Nein|Abfrage|keine |Suche basierend auf Größe, Aspekt, Farbe, Formatvorlage, Smiley oder einer beliebigen Kombination daraus zu filtern. Gültige Werte: <ul><li>Größe: kleinen</li><li>Größe: Mittel</li><li>Größe: Groß</li><li>Größe: Breite: [Breite]</li><li>Größe: Höhe: [Höhe]</li><li>Aspekt: Quadrat</li><li>Aspekt: Wide</li><li>Aspekt: hoch</li><li>Farbe: Farbe</li><li>Einfarbig:</li><li>Formatvorlage: Fotos</li><li>Formatvorlage: Grafiken</li><li>Smiley: Smiley</li><li>Smiley: Hochformat</li><li>Smiley: andere</li></ul><br/><br/>Beispiel: 'Größe: kleinen + Aspekt: Quadrat'|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="search-news"></a>Suchen von Informationen    
Ruft News Ergebnisse von Bing-Suchfeld ein.  
```GET: /News```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage| keine|Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage|keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage|keine |Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage|keine |Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|
|newsSortBy|Zeichenfolge|Nein|Abfrage| keine|Sortierreihenfolge für Ergebnisse. Gültige Werte: <ul><li>Datum</li><li>Relevanz</li></ul> <p>Sortierreihenfolge Datum impliziert absteigender Reihenfolge.</p>|
|newsCategory|Zeichenfolge|Nein|Abfrage| |Kategorie der Informationen zum Eingrenzen der Suche (Beispiel: 'Rt_Business')|
|newsLocationOverride|Zeichenfolge|Nein|Abfrage|keine |Außerkraftsetzung für Erkennung der Bing-Speicherort. Für diesen Parameter ist nur in En-US-amerikanischen Markt verfügbar. Das Format für die Eingabe ist US. /<state /> (Beispiel: ' US. WA ")|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="search-spellings"></a>Suche Rechtschreibung    
Ruft Vorschläge Rechtschreibung.  
```GET: /SpellingSuggestions```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage| keine|So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage| keine|Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage| keine|Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage|keine |Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage|keine |Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="search-related"></a>Zusammenhang suchen    
Ruft zugehöriger Suchergebnisse aus einer Bing-Suchfeld.  
```GET: /RelatedSearch```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage| keine|Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage|keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage|keine |Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage| keine|Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="search-all"></a>Alle suchen    
Ruft alle Websites, Videos, Bilder usw. aus einer Bing-Suchfeld.  
```GET: /CompositeSearch```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Abfrage|Zeichenfolge|Ja|Abfrage|keine |So suchen Sie nach Text (Beispiel: 'Xbox')|
|maxResult|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl der Ergebnisse zurück|
|startOffset|ganze Zahl|Nein|Abfrage|keine |Anzahl der Ergebnisse zu überspringen|
|adultContent|Zeichenfolge|Nein|Abfrage|keine |Erwachsene Inhalt filtern. Gültige Werte: <ul><li>Deaktivieren</li><li>Moderieren</li><li>Strict</li></ul>|
|Markt|Zeichenfolge|Nein|Abfrage|keine |Markt oder Region zum Eingrenzen der Suche (Beispiel: En-US)|
|Länge|Zahl|Nein|Abfrage|keine |Länge (OST / "Westen" Koordinate) zum Eingrenzen der Suche (Beispiel: 47.603450)|
|Breite|Zahl|Nein|Abfrage|keine |Breite (Nord/Süd Koordinate) zum Eingrenzen der Suche (Beispiel:-122.329696)|
|webFileType|Zeichenfolge|Nein|Abfrage|keine |Dateityp zum Eingrenzen der Suche (Beispiel: 'Dokument')|
|videoFilters|Zeichenfolge|Nein|Abfrage|keine |Suche basierend auf Größe, Aspekt, Farbe, Formatvorlage, Smiley oder einer beliebigen Kombination daraus zu filtern.  Gültige Werte: <ul><li>Dauer: kurz</li><li>Dauer: Mittel</li><li>Dauer: lange</li><li>Seitenverhältnis: Standard</li><li>Aspekt: Breitbild</li><li>Lösung: "Niedrig"</li><li>Lösung: Mittel</li><li>Lösung: hoch</li></ul> <br/><br/>Beispiel: 'Dauer: kurz + Auflösung: hoch'|
|videoSortBy|Zeichenfolge|Nein|Abfrage|keine |Sortierreihenfolge für Ergebnisse. Gültige Werte: <ul><li>Datum</li><li>Relevanz</li></ul> <p>Sortierreihenfolge Datum impliziert absteigender Reihenfolge.</p>|
|imageFilters|Zeichenfolge|Nein|Abfrage|keine |Suche basierend auf Größe, Aspekt, Farbe, Formatvorlage, Smiley oder einer beliebigen Kombination daraus zu filtern. Gültige Werte: <ul><li>Größe: kleinen</li><li>Größe: Mittel</li><li>Größe: Groß</li><li>Größe: Breite: [Breite]</li><li>Größe: Höhe: [Höhe]</li><li>Aspekt: Quadrat</li><li>Aspekt: Wide</li><li>Aspekt: hoch</li><li>Farbe: Farbe</li><li>Einfarbig:</li><li>Formatvorlage: Fotos</li><li>Formatvorlage: Grafiken</li><li>Smiley: Smiley</li><li>Smiley: Hochformat</li><li>Smiley: andere</li></ul><br/><br/>Beispiel: 'Größe: kleinen + Aspekt: Quadrat'|
|newsSortBy|Zeichenfolge|Nein|Abfrage|keine |Sortierreihenfolge für Ergebnisse. Gültige Werte: <ul><li>Datum</li><li>Relevanz</li></ul> <p>Sortierreihenfolge Datum impliziert absteigender Reihenfolge.</p>|
|newsCategory|Zeichenfolge|Nein|Abfrage|keine |Kategorie der Informationen zum Eingrenzen der Suche (Beispiel: 'Rt_Business')|
|newsLocationOverride|Zeichenfolge|Nein|Abfrage|keine |Außerkraftsetzung für Erkennung der Bing-Speicherort. Für diesen Parameter ist nur in En-US-amerikanischen Markt verfügbar. Das Format für die Eingabe ist US. /<state /> (Beispiel: ' US. WA ")|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="webresultmodel-bing-web-search-results"></a>WebResultModel: Bing Web-Suchergebnissen

|Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|DisplayUrl|Zeichenfolge|Nein|
|ID|Zeichenfolge|Nein|
|FullUrl|Zeichenfolge|Nein|

#### <a name="videoresultmodel-bing-video-search-results"></a>VideoResultModel: Video Bing-Suchergebnisse

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Nein|
|DisplayUrl|Zeichenfolge|Nein|
|ID|Zeichenfolge|Nein|
|MediaUrl|Zeichenfolge|Nein|
|Laufzeit|ganze Zahl|Nein|
|Miniaturansicht|nicht definiert|Nein|

#### <a name="thumbnailmodel-thumbnail-properties-of-the-multimedia-element"></a>ThumbnailModel: Miniaturansicht-Eigenschaften des Elements multimedia

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|MediaUrl|Zeichenfolge|Nein|
|ContentType|Zeichenfolge|Nein|
|Breite|ganze Zahl|Nein|
|Höhe|ganze Zahl|Nein|
|FileSize|ganze Zahl|Nein|

#### <a name="imageresultmodel-bing-image-search-results"></a>ImageResultModel: Bing Bild Suchergebnisse

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Nein|
|DisplayUrl|Zeichenfolge|Nein|
|ID|Zeichenfolge|Nein|
|MediaUrl|Zeichenfolge|Nein|
|SourceUrl|Zeichenfolge|Nein|
|Miniaturansicht|nicht definiert|Nein|

#### <a name="newsresultmodel-bing-news-search-results"></a>NewsResultModel: Bing News Suchergebnisse

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|DisplayUrl|Zeichenfolge|Nein|
|ID|Zeichenfolge|Nein|
|Datenquelle|Zeichenfolge|Nein|
|Datum|Zeichenfolge|Nein|

#### <a name="spellresultmodel-bing-spelling-suggestions-results"></a>SpellResultModel: Bing Rechtschreibung Vorschläge Ergebnisse

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein|
|Wert|Zeichenfolge|Nein|

#### <a name="relatedsearchresultmodel-bing-related-search-results"></a>RelatedSearchResultModel: Bing Zusammenhang Suchergebnisse

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Nein|
|ID|Zeichenfolge|Nein|
|BingUrl|Zeichenfolge|Nein|

#### <a name="compositesearchresultmodel-bing-composite-search-results"></a>CompositeSearchResultModel: Zusammengesetzte Bing-Suchergebnisse

|Eigenschaftsname | Datentyp |Erforderlich |
|---|---|---|
|WebResultsTotal|ganze Zahl|Nein|
|ImageResultsTotal|ganze Zahl|Nein|
|VideoResultsTotal|ganze Zahl|Nein|
|NewsResultsTotal|ganze Zahl|Nein|
|SpellSuggestionsTotal|ganze Zahl|Nein|
|WebResults|Matrix|Nein|
|ImageResults|Matrix|Nein|
|VideoResults|Matrix|Nein|
|NewsResults|Matrix|Nein|
|SpellSuggestionResults|Matrix|Nein|
|RelatedSearchResults|Matrix|Nein|

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

Kehren Sie zu der [Liste der APIs](apis-list.md).
