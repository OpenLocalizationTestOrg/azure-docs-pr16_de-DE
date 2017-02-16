<properties 
    pageTitle="Verweis in Analytics in Anwendung Einsichten | Microsoft Azure" 
    description="Referenz für Anweisungen in Analytics, die leistungsfähige Suchfunktion der Anwendung Einsichten. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Verweis für Analytics

[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion der [Anwendung Einsichten](app-insights-overview.md). Diese Seiten werden die Analytics-Abfragesprache beschrieben.

> [AZURE.NOTE] [Test Drive Analytics auf unsere simulierten Daten](https://analytics.applicationinsights.io/demo) , wenn Ihre app Anwendung Einsichten noch Senden von Daten nicht zur Verfügung.

## <a name="index"></a>Index


**Zulassen, dass** [zulassen, dass](#let-clause)


**Abfragen und Operatoren** [zählen](#count-operator)  |  [ausgewertet werden](#evaluate-operator) | [Erweitern](#extend-operator) | [Verknüpfung](#join-operator) | [Grenzwert](#limit-operator) | [Mvexpand](#mvexpand-operator) | [Analysieren](#parse-operator) | [Project](#project-operator) | [Projekt abwesend](#project-away-operator) | [Bereich](#range-operator) | [verringern](#reduce-operator) | [Rendern Richtlinie](#render-directive) | [Klausel einschränken](#restrict-clause) | [Sortieren](#sort-operator) | [zusammenfassen](#summarize-operator) | [Ausführen](#take-operator) | [oben](#top-operator) | [oben geschachtelt](#top-nested-operator) | [Union](#union-operator) | [, in dem](#where-operator) | [Where in](#where-in-operator)

**Aggregationen** [Alle](#any)  |  [Argmax](#argmax) | [Argmin](#argmin) | [Mittelwert](#avg) | [Buildschema](#buildschema) | [zählen](#count) | [ZÄHLENWENN](#countif) | [DBANZAHL](#dcount) | [Dcountif](#dcountif) | [Makelist](#makelist) | [Makeset](#makeset) | [max](#max) | [min](#min) | [Quantil](#percentile) | [Quantile zwischen](#percentiles) | [Percentilesw](#percentilesw) | [Percentilew](#percentilew) | [STABW](#stdev) | [Summe](#sum) | [Varianz](#variance)

**Skalare** [Boolesche literalen](#boolean-literals)  |  [Boolesche Operatoren](#boolean-operators) | [wandelt](#casts) | [skalaren Vergleiche](#scalar-comparisons) | [Gettype](#gettype) | [Hash](#hash) | [iff](#iff) | [Isnotnull](#isnotnull) | [Isnull](#isnull) | [Notnull](#notnull) | [Toscalar](#toscalar)

**Zahlen** [Arithmetische Operatoren](#arithmetic-operators)  |  [Numerischen Literalen](#numeric-literals) | [abs](#abs) | [Papierkorb](#bin) | [exp](#exp) | [Untergrenze](#floor) | [Gamma](#gamma) | [Log](#log) | [Rand](#rand) | [Wurzel](#sqrt) | [Todouble](#todouble) | [Toint](#toint) | [Tolong](#tolong)

**Datum und Uhrzeit** [Datum und Uhrzeit Ausdrücke](#date-and-time-expressions)  |  [Datum und Uhrzeit literalen](#date-and-time-literals) | [vor](#ago) | [Datepart](#datepart) | [Dayofmonth](#dayofmonth) | [Wochentag](#dayofweek) | [Dayofyear](#dayofyear) | [Endofday](#endofday) | [Endofmonth](#endofmonth) | [Endofweek](#endofweek) | [Endofyear](#endofyear) | [Getmonth](#getmonth) | [Getyear](#getyear) | [jetzt](#now) | [Startofday](#startofday) | [Startofmonth](#startofmonth) | [Startofweek](#startofweek) | [Startofyear](#startofyear) | [Todatetime](#todatetime) | [Totimespan](#totimespan) | [Weekofyear](#weekofyear)

**Zeichenfolge** [GUIDs](#guids)  |  [Verborgen Zeichenfolgenliteralen](#obfuscated-string-literals) | [Zeichenfolgenliteralen](#string-literals) | [Vergleichen von Zeichenfolgen](#string-comparisons) | [Countof](#countof) | [extrahieren](#extract) | [Isempty](#isempty) | [Isnotempty](#isnotempty) | [Notempty](#notempty)| [Parseurl](#parseurl) | [Ersetzen](#replace) | [Teilen](#split) | [Strcat](#strcat) | [Strlen](#strlen) | [Teilzeichenfolge](#substring) | [Tolower](#tolower) | [Toupper](#toupper)

**Arrays, Objekte und dynamisch** [Matrix und Object-literalen](#array-and-object-literals)  |  [Dynamisches Objektfunktionen](#dynamic-object-functions) | [dynamische Objekte in lassen Klauseln](#dynamic-objects-in-let-clauses) | [JSON-Pfad Ausdrücke](#json-path-expressions) | [Namen](#names) | [Arraylength](#arraylength) | [Extractjson](#extractjson) | [Parsejson](#parsejson) | [Bereich](#range) | [Todynamic](#todynamic) | [Treepath](#treepath)



## <a name="let"></a>Zulassen, dass

### <a name="let-clause"></a>Lassen Sie die Klausel

**Tabellarische lassen: Benennen einer Tabelle**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalarwert lassen – benennen einen Wert**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda lassen: Benennen einer Funktion**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Eine lassen-Klausel bindet einen [Namen](#names) zu einem tabellarischen Ergebnis, Skalarwert oder (Funktion). Die-Klausel ist ein Präfix zu einer Abfrage, und der Bereich der Bindung ist die Abfrage. (Informieren bieten keine Möglichkeit zum Namen Dinge, die Sie später in der Sitzung zu verwenden.)

**Syntax**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *Typ:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *Plain_query:* Eine Abfrage, die eine informieren-Klausel nicht mit dem Präfix.

**Beispiele**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Selbstjoins:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Abfragen und Operatoren

Eine Abfrage über Ihre werden besteht aus einen Verweis auf eine Datenquelle Stream, gefolgt von einer Verkaufspipeline Filter. Beispiel:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Die einzelnen Filter mit dem Präfix des Pipe-Zeichens `|` ist eine Instanz eines *Operators*, mit einigen Parameter. Die Eingabe an den Operator ist die Tabelle, die das Ergebnis der vorherigen Verkaufspipeline ist. In den meisten Fällen sind alle Parameter [skalaren Ausdrücken](#scalars) über die Spalten der Eingabe. In einigen Fällen die Parameter werden die Namen von Spalten, und der Parameter in einigen Fällen ist eine zweite Tabelle. Das Ergebnis einer Abfrage ist immer eine Tabelle, auch wenn sie nur eine Spalte und eine Zeile hat.

Abfragen können einzelne Zeilenumbrüche enthalten, aber Sie werden durch eine Leerzeile beendet. Sie können zwischen Kommentare enthalten `//` und Ende der Zeile.

Eine Abfrage kann eine oder mehrere [Klauseln lassen](#let-clause), vorangestellt werden die definieren skalare, Tabellen oder Funktionen, die innerhalb der Abfrage verwendet werden können.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`wird in den folgenden Beispielen der Abfrage verwendet, um der obigen Tabelle der Verkaufspipeline oder Quelle zu kennzeichnen.
> 

### <a name="count-operator"></a>Count-operator

Die `count` Operator gibt die Anzahl der Datensätze (Zeilen), in die Eingabewerte Datensatzgruppe.

**Syntax**

    T | count

**Argumente auf:**

* *T*: Daten in der Tabelle, deren Datensätze werden gezählt werden soll.

**Gibt**

Diese Funktion gibt eine Tabelle mit einem einzigen Datensatz und eine Spalte vom Typ `long`. Der Wert der Zelle, die nur ist die Anzahl der Datensätze in *T*. 

**Beispiel**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>Auswerten der operator

`evaluate`ist eine Erweiterung Methode, mit dem spezielle Algorithmen zu Abfragen hinzugefügt werden kann.

`evaluate`muss der letzte Operator in der Abfrage Verkaufspipeline (eine Ausnahme bilden jedoch eine mögliche `render`). Es muss eine Funktion Textbereich nicht angezeigt.

[Auswerten Autocluster](#evaluate-autocluster) | [Auswerten Korb](#evaluate-basket) | [Auswerten Diffpatterns](#evaluate-diffpatterns) | [Extractcolumns auswerten](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>Auswerten autocluster

     T | evaluate autocluster()

AutoCluster findet gängige Muster diskrete Attribute (Dimensionen) in den Daten und werden die Ergebnisse der ursprünglichen Abfrage (gibt an, ob sie 100 oder 100 k Zeilen befindet) auf eine kleine Anzahl von Mustern zu verringern. AutoCluster kann auf einer beliebigen gefilterten Datenmenge potenziell funktioniert jedoch wurde entwickelt, um Fehler (z. B. Ausnahmen, stürzt ab) zu analysieren. 

**Syntax**

    T | evaluate autocluster( arguments )

**Gibt**

AutoCluster gibt (normalerweise) wenigen Muster, die Teile der Daten mit freigegebenen allgemeine Werte über mehrere diskrete Attribute zu erfassen. Jedes Muster wird durch eine Zeile in die Ergebnisse dargestellt. 

Die ersten beiden Spalten sind die Anzahl und den Prozentsatz der Zeilen aus der ursprünglichen Abfrage, die mit dem Muster erfasst werden. Die verbleibenden Spalten aus der ursprünglichen Abfrage sind und deren Wert ist, einen bestimmten Wert in der Spalte oder ' *' d. h. Variablenwerten. 

Beachten Sie, dass die Muster nicht nicht zusammenhängenden sind: diese möglicherweise überlappen und bedecken Sie normalerweise nicht alle ursprünglichen Zeilen. Einige Zeilen möglicherweise nicht in einem beliebigen Muster fallen.

**Tipps**

* Verwenden Sie `where` und `project` in der Eingabewerte Pipe, die Daten zu verringern, um nur was Sie interessiert.
* Wenn Sie eine Zeile interessante finden, möglicherweise möchten Sie darin weiteren Ausführen von Drilldowns oder durch Hinzufügen ihrer bestimmte Werte auf Ihre `where` filtern.

**Argumente (optional)**

* `output=all | values | minimal` 

    Das Format der Ergebnisse. Die Anzahl und Prozent Spalten werden immer in den Ergebnissen angezeigt. 

 * `all`-alle Spalten aus der Eingabe werden die Ausgabe
 * `values`– Filtert Spalten, wobei nur ' *' in den Ergebnissen
 * `minimal`– auch filtert Spalten, die für alle Zeilen in der ursprünglichen Abfrage identisch sind. 


* `min_percent=`*doppelte* (Standard: 1)

    Die minimale Prozentsatz Reichweite der generierten Zeilen.

    Beispiel:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*Ganzzahl* (Standard: 25) 

    Die Anzahl der Basis ermittelt die Anzahl von Punkten der ersten lokalen Suche des Algorithmus. In einigen Fällen, abhängig von der Struktur der Daten erhöht die Anzahl der Basis der Zahl (oder Qualität) der Ergebnisse durch höhere Suche Leerzeichen bei langsamer Abfrage Kompromiss. Das Argument Num_seeds weist Schwächen der Ergebnisse in beiden Richtungen, damit es unter 5 unbefriedigend unerheblich leistungsverbesserungen erzielen und zunehmender über 50 wird kaum zusätzliche Muster generieren.

    Beispiel:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < double < 1*+ (Standard: 0,5)

    Bietet Ihnen einige Kontrolle über die Balance zwischen generisch (hoher Schutz) und informativen (viele freigegebenen Werte). Zunehmenden Size_weight normalerweise verringert die Anzahl von Mustern und jedes Muster wird möglicherweise einen höheren Prozentsatz bedecken. Verringern normalerweise Size_weight erzeugt spezifischere Muster mit mehr freigegebenen Werten und kleinere Prozentsatz Schutz. Der Erweiterte Einstellungen Formel wird eine geometrische zwischen standardisierte generische Bewertung und informativen Punktzahl mit Size_weight und 1-Size_weight als die-Stärken. 

    Beispiel:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*Spaltenname*

    Jede Zeile in der Eingabe entsprechend der angegebenen Stärke (standardmäßig weist jede Zeile eine Stärke von "1"), gemeinsame Verwendung einer Spalte Stärke ist zu berücksichtigen, in dem Konto werden oder das Zuordnen von Buckets/Aggregation der Daten, die bereits in jeder Zeile eingebettet ist.

    Beispiel:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>Auswerten Korb

     T | evaluate basket()

Warenkorb findet alle häufige Muster von diskrete Attribute (Dimensionen) in den Daten und alle häufige Muster, die den Schwellenwert der Häufigkeit in der ursprünglichen Abfrage übergeben zurückgegeben werden kann. Korb ist gewährleistet, dass alle häufige Muster in den Daten finden, aber nicht unbedingt polynomische Laufzeit haben. Laufzeit der Abfrage ist die Anzahl der Zeilen lineare jedoch in einigen Fällen sinnvoll sein exponentiellen die Anzahl der Spalten (Dimensionen). Korb basiert auf dem Zufallszahl Algorithmus ursprünglich für Korb Analyse Datamining entwickelt wurden. 

**Gibt**

Alle Muster in mehr als einen angegebenen Bruch (Standard 0,05) der Ereignisse angezeigt werden.

**Argumente (optional)**


* `threshold=`*0.015 < double < 1* (Standard: 0,05) 

    Legt die minimale das Verhältnis zwischen der Zeilen, die häufig verwendeten berücksichtigt werden (Muster mit kleiner das Verhältnis werden nicht zurückgegeben werden).

    Beispiel:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*Spaltenname*

    Jede Zeile in der Eingabe entsprechend der angegebenen Stärke (standardmäßig weist jede Zeile eine Stärke von "1"), gemeinsame Verwendung einer Spalte Stärke ist zu berücksichtigen, in dem Konto werden oder das Zuordnen von Buckets/Aggregation der Daten, die bereits in jeder Zeile eingebettet ist.

    Beispiel: T | Auswerten basket("weight_column=sample_Count")


* `max_dims=`*1 < Int* (Standard: 5)

    Legt die maximale Anzahl von nichtkorrelierte Dimensionen pro Korb, standardmäßig verringern Sie die Abfrage Laufzeit begrenzt.


* `output=minimize` | `all` 

    Das Format der Ergebnisse. Die Anzahl und Prozent Spalten werden immer in den Ergebnissen angezeigt.

 * `minimize`– Filtert Spalten, wobei nur ' *' in den Ergebnissen.
 * `all`-alle Spalten aus der Eingabe werden ausgegeben.




#### <a name="evaluate-diffpatterns"></a>Auswerten diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns vergleicht zwei Datasets derselben Struktur und findet Mustern diskrete Attribute (Dimensionen), die Unterschiede zwischen den beiden angegebenen Datengruppen kennzeichnen. Diffpatterns wurde entwickelt, um Fehler (z. B. durch Vergleichen von Fehlern auf Fehler in einen bestimmten zeitlichen Rahmen) analysieren, aber potenziell Unterschiede zwischen einer beliebigen zweier Reihen von merkmalsausprägungen derselben Struktur finden. 

**Syntax**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Gibt**

Diffpatterns gibt (normalerweise) wenigen Muster, die anderen Teile der Daten in den beiden Mengen (d. h. ein Muster erfassen einen großen Prozentsatz der Zeilen in der ersten Gruppe von Daten und niedrig Prozentsatz der Zeilen in der zweiten) erfassen. Jedes Muster wird durch eine Zeile in die Ergebnisse dargestellt.

Die ersten vier Spalten sind die Anzahl und den Prozentsatz der Zeilen aus der ursprünglichen Abfrage, die mit dem Muster in jedem Satz erfasst werden, die fünfte Spalte ist der Unterschied (in absolute Prozentpunkte) zwischen den beiden Mengen. Die verbleibenden Spalten aus der ursprünglichen Abfrage sind und deren Wert ist, einen bestimmten Wert in der Spalte oder * d. h. Variablenwerten. 

Beachten Sie, dass die Muster nicht distinct sind: diese möglicherweise überlappen und bedecken Sie normalerweise nicht alle ursprünglichen Zeilen. Einige Zeilen möglicherweise nicht in einem beliebigen Muster fallen.

**Tipps**

* Verwenden von Where und project in der Eingabewerte Pipe, die Daten zu verringern, um nur was Sie interessiert.

* Wenn Sie eine interessante Zeile gefunden haben, sollten Sie detailliert auswerten weiter durch die spezifischen Werten zum Filtern der Where hinzufügen.

**Argumente auf:**

* `split=`*Spaltenname* (erforderlich)

    Die Spalte muss exakt zwei Werte haben. Erstellen Sie bei Bedarf eine solche Spalte:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*Zeichenfolge*

    Erfahren, der Algorithmus zur Überprüfung nur Muster der höheren Prozentsatz aus der Datenmenge Ziel haben, die das Ziel muss eine der zwei Werte in der Spalte teilen.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < double < 1* (Standard: 0,05) 

    Legt den minimalen Muster (Verhältnis) Unterschied zwischen den zwei Sätzen.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Das Format der Ergebnisse. Die Anzahl und Prozent Spalten werden immer in den Ergebnissen angezeigt. 

 * `minimize`– Filtert Spalten, wobei nur ' *' in den Ergebnissen
 * `all`-alle Spalten aus der Eingabe werden die Ausgabe

* `weight_column=`*Spaltenname*

    Jede Zeile in der Eingabe entsprechend der angegebenen Stärke (standardmäßig weist jede Zeile eine Stärke von "1"). Eine häufige Verwendung einer Spalte Stärke ist zu berücksichtigen, in dem Konto werden oder das Zuordnen von Buckets/Aggregation der Daten, die bereits in jeder Zeile eingebettet ist.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>Auswerten extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns wird verwendet, um eine Tabelle mit mehreren einfachen Spalten bereichern, die außerhalb des (halb) dynamisch extrahierten strukturierten Spalte(n) basierend auf deren sind. Unterstützt derzeit nur Json-Spalten beide Dynamic Zeichenfolgenserialisierung von Jsons.


* `max_columns=`*Ganzzahl* (Standard: 10) 

    Die Anzahl der neuen hinzugefügten Spalten ist dynamisch und kann sehr groß (tatsächlich ist die Anzahl der distinct-Schlüssel in allen Json-Datensätzen) sein, damit wir eingeschränkt werden muss. Die neuen Spalten basierend auf deren Häufigkeit in absteigender Reihenfolge sortiert sind und nach Zeitphasen bis zum Max_columns zur Tabelle hinzugefügt werden.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*doppelte* (Standard: 10.0) 

    Alternativ können Sie neuen Spalten einschränken, indem Sie Spalten ignorieren, deren Häufigkeit ist niedriger als Min_percent.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*bool* (Standard: WAHR) 

    Ist der Wert true wird der Name der Spalte komplexer als ein Präfix den Namen des extrahierten Spalten hinzugefügt werden.


* `prefix_delimiter=`*Zeichenfolge* (Standard: "_") 

    Wenn Add_prefix = WAHR für diesen Parameter definiert das Trennzeichen, das auf die Namen der neuen Spalten verketten verwendet wird.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*bool* (Standard: falsch) 

    Ist der Wert true werden die ursprünglichen (Json) Spalten in der Ausgabetabelle beibehalten.


* `output=query | table` 

    Das Format der Ergebnisse. 

 * `table`-Die Ausgabe ist der gleichen Tabelle aus, wie empfangen minus der angegebenen Eingabewerte Spalten sowie neue Spalten, die aus der Eingabewerte Spalten extrahiert wurden.
 * `query`-Die Ausgabe ist eine Zeichenfolge, die Abfrage, die Sie vornehmen möchten, um das Ergebnis als Tabelle darstellt. 




### <a name="extend-operator"></a>Erweitern der operator

     T | extend duration = stopTime - startTime

Anfügen einer oder mehreren berechneter Spalten zu einer Tabelle. 


**Syntax**

    T | extend ColumnName = Expression [, ...]

**Argumente auf:**

* *T:* Die Eingabewerte Tabelle.
* *ColumnName:* Der Name einer Spalte hinzufügen. [Namen](#names) Groß-/Kleinschreibung und dürfen alphabetisch, numerisch oder '_' Zeichen. Verwenden Sie `['...']` oder `["..."]` Zitat Schlüsselwörter oder Namen mit anderen Zeichen.
* *Ausdruck:* Eine Berechnung über die vorhandenen Spalten.

**Gibt**

Eine Kopie der Eingabewerte Tabelle mit den angegebenen weiteren Spalten.

**Tipps**

* Verwenden Sie [`project`](#project-operator) stattdessen, wenn Sie auch löschen oder Umbenennen von einige Spalten möchten.
* Verwenden Sie keine `extend` einfach, zum Abrufen eines verkürzen Namens in einem langen Ausdruck verwenden. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Die native Spalten der Tabelle haben indiziert wurde. der neue Name definiert eine zusätzliche Spalte, die bei einem nicht indizierten, damit die Abfrage wahrscheinlich langsamer ausgeführt wird.

**Beispiel**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>Join-operator

    Table1 | join (Table2) on CommonColumn

Die Werte der angegebenen Spalte zusammengeführt die Zeilen von zwei Tabellen.


**Syntax**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumente auf:**

* *Tabelle1* - die 'linke Seite' der Verknüpfung.
* *Tabelle2* - 'rechts' der Verknüpfung. Es kann ein Ausdruck geschachtelte Abfrage sein, der eine Tabelle ausgegeben.
* *CommonColumn* - eine Spalte mit demselben Namen in den beiden Tabellen aus.
* *Art* - gibt an, wie Zeilen aus beiden Tabellen abgeglichen werden.

**Gibt**

Eine Tabelle mit:

* Eine Spalte für jede Spalte in jeder der beiden Tabellen, einschließlich der übereinstimmenden Schlüsseln. Die Spalten des rechten Rands werden automatisch umbenannt werden, wenn ein Konflikt vorhanden sind.
* Eine Zeile für jede Übereinstimmung zwischen den Tabellen Eingabewerte. Eine Übereinstimmung ist eine Zeile aus einer Tabelle mit dem gleichen Wert für alle ausgewählten der `on` Felder als eine Zeile in der anderen Tabelle. 

* `Kind`nicht angegeben

    Nur eine Zeile aus der linken Seite wird für jeden Wert von abgeglichen die `on` Schlüssel. Die Ausgabe enthält eine Zeile für jede Übereinstimmung mit dieser Zeile mit Zeilen von rechts.

* `Kind=inner`
 
     In der Ausgabe für jede Kombination von übereinstimmende Zeilen ist von links und rechts eine Zeile vorhanden.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Zusätzlich zu der innere entspricht ist eine Zeile für jede Zeile auf der linken (und/oder rechts), auch wenn es keine Entsprechung vorhanden ist. Die Ausgabe zur Inkonsistenzsuche Zellen wird in diesem Fall NULL-Werte enthalten.

* `kind=leftanti`

     Gibt alle Datensätze von der linken Seite, die keine Übereinstimmung von rechts haben. Die Ergebnistabelle enthält nur die Spalten von der linken Seite. 
 
Wenn Sie mehrere Zeilen mit denselben Werten für die betreffenden Felder vorhanden sind, erhalten Sie Zeilen für alle Kombinationen.

**Tipps**

Zur Optimierung der Systemleistung:

* Verwenden Sie `where` und `project` die Nummern der Zeilen und Spalten in die Eingabewerte Tabellen, bevor Verringern der `join`. 
* Wenn Sie eine Tabelle immer kleiner ist als die andere ist, als (piped) links von der Verknüpfung verwenden.
* Die Spalten für die Verknüpfung Übereinstimmung müssen den gleichen Namen. Verwenden Sie den Operator Projekt aus, falls notwendig Umbenennen eine Spalte in einer Tabelle.

**Beispiel**

Abrufen von erweiterten Sie Aktivitäten aus einem Protokoll, in welche einige Einträge kennzeichnen Anfang und Ende einer Aktivität. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>Grenzwert für operator

     T | limit 5

Gibt auf die angegebene Anzahl von Zeilen aus einer Tabelle. Es gibt keine Garantie, welche Datensätze zurückgegeben werden. (Verwenden, um bestimmte Datensätze zurückzugeben [`top`](#top-operator).)

**Alias**`take`

**Syntax**

    T | limit NumberOfRows


**Tipps**

`Take`ist eine einfache und effiziente Möglichkeit, ein Beispiel für die Ergebnisse angezeigt, wenn Sie interaktiv arbeiten. Achten Sie darauf, dass sie um bestimmten Zeilen zu erstellen oder zu erzeugen, diese in keiner bestimmten Reihenfolge gewährleistet wird nicht.

Ist ein implizit begrenzt die Anzahl der Zeilen, die an den Client, zurückgegeben, auch wenn Sie nicht verwenden `take`. Um diese Beschränkung heben, verwenden Sie die `notruncation` Client Anforderung Option.



### <a name="mvexpand-operator"></a>Mvexpand-operator

    T | mvexpand listColumn 

Eine Liste aus einer Dynamic eingegeben (JSON) Zelle erweitert, damit jeder Eintrag eine separate Zeile hat. Alle Zellen in einer Zeile erweitert werden dupliziert. 

(Siehe auch [`summarize makelist`](#summarize-operator) der entgegengesetzte-Funktion vornimmt.)

**Beispiel**

Angenommen Sie, die Eingabewerte Tabelle ist:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hallo"|{"Key": "Wert"}|
|2|"www"|[0,1, "k", "w"]|

    mvexpand D

Ergebnis lautet:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hallo"|{"Key": "Wert"}|
|2|"www"|0|
|2|"www"|1|
|2|"www"|"k"|
|2|"www"|"w"|


**Syntax**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumente auf:**

* *ColumnName:* Im Ergebnis werden in der benannten Spalte Matrizen in mehreren Zeilen erweitert. 
* *ArrayExpression:* Ein Ausdruck, so dass einer Matrix zurück. Wenn dieses Formular verwendet wird, wird eine neue Spalte hinzugefügt, und die vorhandenen beibehalten.
* *Name:* Einen Namen für die neue Spalte.
* *Typename:* Wandelt den erweiterten Ausdruck in einen bestimmten Typ
* *RowLimit:* Die maximale Anzahl von Zeilen aus jeder Zeile der ursprünglichen generiert. Die Standardeinstellung ist 128.

**Gibt**

Mehrere Zeilen für jede der Werte in eine Matrix in der benannten Spalte oder im Ausdruck Matrix zurück.

Die erweiterte Spalte enthält immer dynamischen Typ aus. Verwenden Sie eine Umwandlung wie `todatetime()` oder `toint()` berechnen oder Aggregieren von Werten werden soll.

Zwei Modi Eigenschaft-Sammlung Expansions werden unterstützt:

* `bagexpansion=bag`: Eigenschaft Sammlungen werden in den einzelnen-Eintrag Eigenschaftensammlungen erweitert. Dies ist die Standard-Erweiterung.
* `bagexpansion=array`: Eigenschaft Sammlungen werden in zwei Elementen erweitert `[` *Schlüssel*`,`*Wert* `]` array Strukturen, gleicht einheitlichen Zugriff auf Schlüssel und Werte (ebenso wie, beispielsweise eine Aggregation distinct Count über Eigenschaftennamen ausführen). 

**Beispiele**


    exceptions | take 1 
  	| mvexpand details[0]

Teilt einen Ausnahmedatensatz in Zeilen für jedes Element in das Feld "Details".



### <a name="parse-operator"></a>Analysieren Sie operator

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Extrahiert Werte aus einer Zeichenfolge an. Können einfache oder regulären Ausdrücken verwenden.

**Syntax**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumente auf:**

* `T`: Die Eingabewerte Tabelle.
* `kind`: 
 * `simple`(Standard): die `Match` Zeichenfolgen einfarbigen Zeichenfolgen sind.
 * `relaxed`: Wenn der Text nicht als den Datentyp einer Spalte analysiert werden, wird die Spalte auf Null und die Analyse festgelegt weiterhin 
 * `regex`: die `Match` Zeichenfolgen sind reguläre Ausdrücke.
* `Text`: Eine Spalte oder eines anderen Ausdrucks, die ergibt oder in einer Zeichenfolge konvertiert werden kann.
* *Übereinstimmung:* Den nächsten Teil die Zeichenfolge entsprechen, und ihn verwerfen.
* *Spalte:* Weisen Sie den nächsten Teil die Zeichenfolge auf diese Spalte ein. Die Spalte wird erstellt, wenn er nicht vorhanden ist.
* *Typ:* Analysieren Sie den nächsten Teil die Zeichenfolge mit dem angegebenen Typ, z. B. Int, Datum, Doppeltippen. 


**Gibt**

Die Eingabewerte Tabelle, entsprechend der Liste der Spalten erweitert.

Die Elemente in der `with` Klausel sind wiederum mit den Quelltext abgeglichen. Jedes Element chews Deaktivieren eines Abschnitts des Quelltexts: 

* Eine Zeichenfolge oder reguläre Ausdrücke verschiebt den übereinstimmenden Cursor durch die Länge der Übereinstimmung aus.
* Eine reguläre Ausdrücke können in eine Regex die Analyse, den Operator Minimierung '?', wechseln Sie zur folgenden Übereinstimmung so früh wie möglich.
* Ein Spaltenname mit einem Typ analysiert den Text, dem angegebenen Typ. Es sei denn, Art = niedrige, eine nicht erfolgreich analysieren macht das gesamte Muster entsprechen.
* Ein Spaltenname ohne einen Typ oder mit dem Typ "String", übernimmt die minimale Anzahl von Zeichen, um die folgenden Übereinstimmung zu gelangen.
* ' *' Überspringt die minimale Anzahl von Zeichen, um die folgenden Übereinstimmung zu gelangen. Sie können '*' am Anfang und Ende des Musters, oder hinter ein anderes als Zeichenfolge oder zwischen Zeichenfolge entspricht.

Alle Elemente in einem Muster analysieren müssen ordnungsgemäß übereinstimmen; Andernfalls werden keine Ergebnisse erzeugt werden. Mit dieser Regel eine Ausnahme ist, wenn Kind = niedrige, wenn eine eingegebene Variable Analyse fehlschlägt, wird die restlichen die Analyse fortgesetzt.

**Beispiele**

*Einfache:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Indikator | Präsentieren | Jahr
---|---|---|---
1 | 2 | SOCKS | 63

*Gelockert:*

Wenn die Eingabe eine vollständige Übereinstimmung für jede Spalte eingegebenen enthält, erzeugt eine niedrige die Analyse die gleichen Ergebnisse wie eine einfache die Analyse aus. Aber wenn Sie eine der eingegebenen Spalten nicht ordnungsgemäß analysieren, weiterhin eine niedrige die Analyse im weiteren Verlauf des Musters zu verarbeiten, während eine einfache die Analyse angehalten wird und keine Ergebnis generieren.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | Präsentieren | Jahr
---|---|---
1 |  SOCKS | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

Ressource | Segment | Sperren | Lassen Sie wieder los | vorherigen
---|---|---|---|---
Scheduler | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>Project-operator

    T | project cost=price*quantity, price

Markieren Sie die Spalten enthalten, umbenennen oder löschen und fügen Sie neue berechnete Spalten ein. Die Reihenfolge der Spalten im Ergebnis wird durch die Reihenfolge der Argumente angegeben. Nur die Spalten, die in den Argumenten angegeben sind im Ergebnis enthalten: alle anderen in der Eingabe werden gelöscht.  (Siehe auch `extend`.)


**Syntax**

    T | project ColumnName [= Expression] [, ...]

**Argumente auf:**

* *T:* Die Eingabewerte Tabelle.
* *ColumnName:* Der Name einer Spalte in der Ausgabe angezeigt werden. Wenn kein *Ausdruck*vorhanden ist, muss eine Spalte mit dem Namen in der Eingabe angezeigt. [Namen](#names) Groß-/Kleinschreibung und dürfen alphabetisch, numerisch oder '_' Zeichen. Verwenden Sie `['...']` oder `["..."]` Zitat Schlüsselwörter oder Namen mit anderen Zeichen.
* *Ausdruck:* Optional Skalarausdruck verweisen auf die Eingabewerte Spalten. 

    Es ist zulässig, um eine neue berechnete Spalte mit demselben Namen wie eine vorhandene Spalte in der Eingabe zurückzukehren.

**Gibt**

Eine Tabelle, die mit dem Namen als Argumente und so viele Spalten hat Zeilen wie die Eingabewerte Tabelle.

**Beispiel**

Das folgende Beispiel zeigt verschiedene Arten von Bearbeitung, die ausgeführt werden können mithilfe der `project` Operator. Die Eingabewerte Tabelle `T` hat drei Spalten vom Typ `int`: `A`, `B`, und `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>Project-abwesend operator

    T | project-away column1, column2, ...

Ausschließen von angegebenen Spalten. Das Ergebnis enthält die Eingabewerte Spalten mit Ausnahme derjenigen, die Sie einen Namen.

### <a name="range-operator"></a>Bereichsoperator

    range LastWeek from ago(7d) to now() step 1d

Generiert eine einspaltige Tabelle mit Werten. Beachten Sie, dass es ein Eingabemethoden Verkaufspipeline aufweist. 

|Letzte Woche|
|---|
|2015-12-05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|2015-12-12 09:10:04.627|



**Syntax**

    range ColumnName from Start to Stop step Step

**Argumente auf:**

* *ColumnName:* Der Name der einzelnen Spalte in der Ausgabetabelle.
* *Starten:* Der kleinste Wert in der Ausgabe.
* *Beenden:* Den höchsten Wert in der Ausgabe (oder einer Grenze auf den höchsten Wert, wenn *Schritt* über diesen Wert Schritte) generiert wird.
* *Schritt:* Die Differenz zwischen zwei aufeinander folgenden Werte. 

Als Argumente müssen Zahlen-, Datums-oder Timespan. Sie können nicht die Spalten in einer beliebigen Tabelle verweisen. (Wenn Sie den Bereich auf der Grundlage einer Eingaben Tabelle berechnen möchten, verwenden Sie den [Bereich *(Funktion)*](#range), vielleicht mit dem [Operator Mvexpand](#mvexpand-operator).) 

**Gibt**

Eine Tabelle mit einer einzelnen Spalte namens *ColumnName*, deren Werte zu *Starten*, *Starten Sie sind* + *Schritt*... bis zu und einschließlich *Beenden*.

**Beispiel**  

```AIQL
range Steps from 1 to 8 step 3
```

Eine Tabelle mit einer einzelnen Spalte namens `Steps` handelt, deren `long` und dessen Werte `1`, `4`, und `7`.

**Beispiel**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Eine Tabelle mit Mitternacht bei der letzten sieben Tage. Die Funktion Papierkorb (Floor) wird jedes Mal an den Anfang des Tages verringert.

**Beispiel**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Zeigt, wie die `range` Operator zum Erstellen eines kleinen, Ad-hoc-der Dimensionstabelle dann Nullen eine Einführung in die Quelldaten hat, in dem keine Werte verwendet werden kann.

### <a name="reduce-operator"></a>Verringern der operator

    exceptions | reduce by outerMessage

Versucht, die ähnliche Datensätze zu gruppieren. Für jede Gruppe, der Operator gibt die `Pattern` sie der Meinung am besten beschreibt die Gruppe, und die `Count` von Datensätzen in dieser Gruppe.


![](./media/app-insights-analytics-reference/reduce.png)

**Syntax**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumente auf:**

* *ColumnName:* Die Spalte zu untersuchen. Es muss vom Zeichenfolgentyp sein.
* *Schwellenwert:* Ein Wert im Bereich {von 0.. 1}. 0,001 der Standardwert liegt. Bei großen Eingaben sollte Schwellenwert klein sein. 

**Gibt**

Zwei Spalten, `Pattern` und `Count`. In vielen Fällen werden Muster eine vollständige Wert aus der Spalte aus. In einigen Fällen können sie häufig verwendete Begriffe identifizieren und Ersetzen Sie die Variablen Teile mit ' *'.

Beispielsweise das Ergebnis des `reduce by city` , enthalten möglicherweise: 

|Muster | Zählen |
|---|---|
| SAN * | 5182 |
| Saint * | 2846 |
| Moskau | 3726 |
| \*-auf-\* | 2730 |
| Paris | 27163 |


### <a name="render-directive"></a>Richtlinie Rendern

    T | render [ table | timechart  | barchart | piechart ]

Rendern weist der Präsentationsebene die Tabelle anzeigen. Es sollte das letzte Element der Pipe sein. Es ist eine geeignete Alternative zu mit den Steuerelementen auf die Anzeige, so dass Sie eine Abfrage mit einer bestimmten Präsentation Methode zu speichern.

### <a name="restrict-clause"></a>Einschränken der Klausel 

Gibt den Satz von Tabellennamen Operatoren, die Folgen zur Verfügung. Beispiel:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>Sortieren-operator 

    T | sort by country asc, price desc

Sortieren Sie die Zeilen der Tabelle Eingabewerte in der Reihenfolge an, indem eine oder mehrere Spalten.

**Alias**`order`

**Syntax**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumente auf:**

* *T:* Die Tabelle Eingabemethoden um zu sortieren.
* *Spalte:* Spalte mit *T* nach der sortiert werden soll. Der Typ der Werte muss numerischen, Datum, Uhrzeit oder Zeichenfolge an.
* `asc`Sortieren nach in aufsteigender Reihenfolge, niedrig zu hoch. Die Standardeinstellung ist `desc`, absteigend hoch zu niedrig.

**Beispiel**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Alle Zeilen in der Tabelle auf, die eine bestimmten haben `ActivityId`, sortiert nach deren Zeitstempel.

### <a name="summarize-operator"></a>Summarize-operator

Erzeugt eine Tabelle, die den Inhalt der Eingabewerte Tabelle fasst zusammen.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Eine Tabelle, die die Anzahl, Durchschnitt Anforderungsdauer und Festlegen von Orten in jedem Land anzeigt. Eine Zeile ist in der Ausgabe für jedes Land distinct vorhanden. Die Ausgabespalten zeigen die Anzahl, Durchschnitt Dauer, Orten und das Land an. Alle anderen Eingabewerte Spalten werden ignoriert.


    T | summarize count() by price_range=bin(price, 10.0)

Eine Tabelle, die zeigt, wie viele Elemente in den einzelnen Intervallen [0,10.0], Preisen haben [10.0,20.0], und so weiter. In diesem Beispiel verfügt über eine Spalte für die Anzahl und eine für den Preisbereich. Alle anderen Eingabewerte Spalten werden ignoriert.


**Syntax**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumente auf:**

* *Spalte:* Optionaler Name für eine Ergebnisspalte. Standardwerte auf einen Namen, die von den Ausdruck abgeleitet werden. [Namen](#names) Groß-/Kleinschreibung und dürfen alphabetisch, numerisch oder '_' Zeichen. Verwenden Sie `['...']` oder `["..."]` Zitat Schlüsselwörter oder Namen mit anderen Zeichen.
* *Aggregation:* Anruf an eine andere Aggregatfunktion wie `count()` oder `avg()`, mit den Spaltennamen als Argumente. Finden Sie unter [Aggregationen](#aggregations).
* *GroupExpression:* Einen Ausdruck über die Spalten, die eine Reihe von unterschiedlichen Werte bereitstellt. Es ist in der Regel entweder einen Spaltennamen ein, der bereits einen eingeschränkten Satz von Werten, bietet oder `bin()` mit einer Spalte für numerische oder Zeit als Argument. 

Wenn Sie einen numerischen oder Time Ausdruck ohne Bereitstellen `bin()`, Analytics wendet es automatisch mit einem Intervall von `1h` für Zeiträume, oder `1.0` für Zahlen.

Wenn Sie eine *GroupExpression,* nicht angeben, wird die gesamte Tabelle in einer einzelnen Ausgabezeile zusammengefasst.



**Gibt**

Die Eingabewerte Zeilen in Gruppen haben Sie die gleichen Werte angeordnet sind die `by` Ausdrücke. Die angegebene Aggregationsfunktionen werden dann über jede Gruppe, die Erstellung einer Zeile für jede Gruppe berechnet. Enthält das Ergebnis der `by` Spalten und auch mindestens eine Spalte für jede berechnet Aggregat. (Einige Aggregationsfunktionen zurückgeben mehrerer Spalten.)

Das Ergebnis hat, sind verschiedene Kombinationen der Anzahl von Zeilen `by` Werte. Wenn Sie über die Bereiche der numerischen Werte zusammenfassen möchten, verwenden Sie `bin()` Bereiche auf diskrete Werte zu reduzieren.

**Notiz**

Obwohl Sie beliebiger Ausdrücke für die Aggregation und die Gruppierungsausdrücke bereitstellen können, ist es effizienter, verwenden Sie einfache Spaltennamen oder anwenden `bin()` einer numerischen Spalte.



### <a name="take-operator"></a>Optimieren der operator

Alias der [Grenzwert](#limit-operator)


### <a name="top-operator"></a>Top-operator

    T | top 5 by Name desc nulls first

Gibt die ersten *N* Datensätze, die durch die angegebenen Spalten sortiert.


**Syntax**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumente auf:**

* *NumberOfRows:* Die Anzahl der Zeilen von *T* , um zurückzukehren.
* *Sort_expression:* Ein Ausdruck, um die Zeilen zu sortieren. Es ist in der Regel nur einen Spaltennamen ein. Sie können mehrere Sort_expression angeben.
* `asc`oder `desc` (die Standardeinstellung) scheint zu steuern, ob die Auswahl aus dem "Bottom" oder "Anfang" des Bereichs tatsächlich ist.
* `nulls first`oder `nulls last` Steuerelemente, wo null-Werte angezeigt werden. `First`ist der Standardwert für `asc`, `last` ist der Standardwert für `desc`.


**Tipps**

`top 5 by name`ersten Blick entspricht dem `sort by name | take 5`. Jedoch schneller ausgeführt und immer gibt Ergebnisse sortiert, während die `take` nicht garantiert ist.

### <a name="top-nested-operator"></a>verschachtelte Top operator

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Erzeugt hierarchische Ergebnisse, wobei jede Ebene einen Drilldown zur vorherigen Ebene ist. Es empfiehlt sich für die Beantwortung von Fragen, die Aussprache wie "Was die oberen 5 Anfragen sind, für jede von ihnen, was die oberen 3 Leistungsgruppen sind und für jede von ihnen, die sind die oberen 3 Länder die Anforderungen kommen aus?"

**Syntax**

   T | Top-geschachtelt N der Spalte anhand der AGGREGATION [,...]

**Argumente auf:**

* N:int - Anzahl von Zeilen zurückgeben, oder auf die nächste Ebene übergeben. In einer Abfrage mit drei Ebenen, wobei N 3, 3 und 5 ist, werden die Gesamtzahl der Zeilen 45 aus.
* Spalte - einer Spalte auf Gruppieren nach zur Aggregation. 
* AGGREGATION - eine [Aggregatfunktion](#aggregations) für jede Gruppe von Zeilen gelten. Die Ergebnisse der folgenden Aggregationen bestimmt die oberen Gruppen angezeigt werden.


### <a name="union-operator"></a>Union-operator

     Table1 | union Table2, Table3

Zwei oder mehr Tabellen und gibt die Zeilen aller Folien. 

**Syntax**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumente auf:**

* *Tabelle1*, *Tabelle2* ...
 *  Der Name einer Tabelle, wie `requests`, oder eine Tabelle in eine [Klausel lassen](#let-clause); definiert oder
 *  Ein Ausdruck, wie Abfragen`(requests | where success=="True")`
 *  Eine Reihe von Tabellen mit einem Platzhalter angegeben. Beispielsweise `e*` bilden die Union aller Tabellen in vorherigen lassen Klauseln, deren Namen mit 'e', zusammen mit der Ausnahmentabelle '' Beginn, definiert.
* `kind`: 
 * `inner`– Das Ergebnis weist die Teilmenge der Spalten, die alle von Tabellen feldbezogene.
 * `outer`– Das Ergebnis enthält alle Spalten, die auftreten in einem der Eingaben an. Zellen, die nicht zeilenweise zeichnen eine Eingabe definiert wurden festgelegt sind, dass `null`.
* `withsource=`*ColumnName:* Wenn angegeben haben, werden die Ausgabe eine Spalte namens *ColumnName* , dessen Wert gibt an, welche Quelltabelle jede Zeile stammen.

**Gibt**

Eine Tabelle mit der Anzahl von Zeilen wie es stehen in allen von Tabellen, und wie viele Spalten wie eindeutigen Spaltennamen in die Eingaben.

**Beispiel**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Union aller Tabellen, deren Namen mit "Tt" beginnen.


**Beispiel**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Die Anzahl der unterschiedlichen Benutzer, die entweder erzeugt sind eine `exceptions` Ereignis oder eine `traces` Ereignis über den letzten Tag. Das Ergebnis wird in der Spalte 'SourceTable' entweder "Abfrage" und "Befehl" angegeben werden.

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Diese Version effizienter zu demselben Ergebnis. Es filtert jede Tabelle vor dem Erstellen der Union ein.

### <a name="where-operator"></a>Wo Operator

     requests | where resultCode==200

Filtert eine Tabelle auf die Teilmenge von Zeilen, die einem Prädikat erfüllen.

**Alias**`filter`

**Syntax**

    T | where Predicate

**Argumente auf:**

* *T:* Tabellarische Eingabe, deren Datensätze gefiltert werden sollen.
* *Prädikat:* A `boolean` [Ausdruck](#boolean) über die Spalten der *T*. Es wird für jede Zeile in *T*ausgewertet.

**Gibt**

Zeilen in *T* für die *Prädikat* ist `true`.

**Tipps**

Um die schnellste Leistung zu erhalten:

* **Verwenden Sie einfache Vergleiche** zwischen Spaltennamen und Konstanten. (Konstante 'Konstante' über die Tabelle - bedeutet, damit `now()` und `ago()` sind in Ordnung, und daher werden skalarer Werte zugewiesen mithilfe einer [ `let` Klausel](#let-clause).)

    Beispielsweise lieber `where Timestamp >= ago(1d)` auf `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest Begriffe ersten**: Wenn Sie mehrere Klauseln mit verbundene haben `and`, setzen zuerst die Klauseln, die nur eine Spalte umfassen. Also `Timestamp > ago(1d) and OpId == EventId` ist besser als die anderen erfolgreichen.


**Beispiel**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Datensätze, die nicht älter als 1 Stunde, stammen aus der Datenquelle mit der Bezeichnung "Kuskus" und besteht aus zwei Spalten mit dem gleichen Wert. 

Beachten Sie, dass wir den Vergleich zwischen zwei Spalten letzten, wie sie den Index kann nicht nutzen und einen Scan erzwingt setzen.


### <a name="where-in-operator"></a>WHERE in-operator

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Syntax**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumente auf:**

* `col`: Eine Spalte in der Tabelle.
* `expr1`...: Eine Liste von skalaren Ausdrücken.

Verwenden Sie `in` wird verwendet, um nur die Zeilen in der einschließen `col` ist gleich der Ausdrücke `expr1...`.

Verwenden Sie `!in` nur die Zeilen in der aufnehmen möchten `col` ist nicht gleich keinem der Ausdrücke `expr1...`.  


## <a name="aggregations"></a>Aggregationen

Aggregationen sind Funktionen verwendet, um Werte in Gruppen in der [summarize-Vorgang](#summarize-operator)erstellt kombinieren. In dieser Abfrage ist beispielsweise dcount() eine Aggregatfunktion an:

    requests | summarize dcount(name) by success

### <a name="any"></a>alle 

    any(Expression)

Zufällig wählt eine Zeile mit der Gruppe aus, und gibt den Wert des angegebenen Ausdrucks zurück.

Dies empfiehlt sich beispielsweise, wenn einige Spalte weist eine große Anzahl von ähnlichen Werten (z. B. eine Spalte "Fehlertext"), und Sie die Spalte einmal pro einen eindeutigen Wert des Gruppe zusammengesetzten Schlüssels aufnehmen möchten. 

**Beispiel**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>Argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Sucht eine Zeile in der Gruppe, die *ExprToMaximize*minimiert/maximiert, und gibt den Wert von *ExprToReturn* (oder `*` , um die gesamte Zeile zurückzukehren).

**Tipp**: die übergeben bis Spalten werden automatisch umbenannt. Um sicherzustellen, dass Sie die richtigen Namen verwenden, prüfen Sie die Ergebnisse mit `take 5` , bevor Sie die Ergebnisse in einem anderen Operator leiten.

**Beispiele**

Zeigen Sie für jeden Anforderungsnamen an, wann die längste Anforderung aufgetreten sind:

    requests | summarize argmax(duration, timestamp) by name

Zeigen Sie aller Details der längsten Anforderung, nicht nur der Zeitstempel an an:

    requests | summarize argmax(duration, *) by name


Suchen Sie den niedrigsten Wert jeder Metrik, zusammen mit ihr Zeitstempel und anderen Daten:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>Mittelwert

    avg(Expression)

Berechnet den Mittelwert der *Ausdruck* in der Gruppe an.

### <a name="buildschema"></a>Buildschema

    buildschema(DynamicExpression)

Gibt das minimale Schema, das alle Werte *DynamicExpression*den Zutritt gewährt. 

Der Parameter Spaltentyp sollten `dynamic` – ein als Matrix oder Eigenschaft Sammlung. 

**Beispiel**

    exceptions | summarize buildschema(details)

Ergebnis:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Beachten Sie, dass `indexer` wird verwendet, um zu kennzeichnen, in dem Sie einen numerischen Index verwendet werden sollen. Für dieses Schema einige gültige Pfade möchten (vorausgesetzt werden, dass diese Indizes Beispiel Bereich aufweisen):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Beispiel**

Angenommen Sie, die Eingabewerte Spalte drei dynamische Werte hat:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Das resultierende Schema wäre:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Das Schema gibt an, die:

* Im Stamm-Objekt ist ein Container mit vier Eigenschaften, die mit dem Namen X, y, Z und t.
* Die Eigenschaft "x", das konnte vom Typ "Int" oder vom Typ "String" sein.
* Die Eigenschaft "y", die entweder vom Typ "double" konnte mit dem Namen oder einen anderen Container mit einer Eigenschaft vom Typ "Zeichenfolge" "w" bezeichnet.
* Die ``indexer`` Schlüsselwort Gibt an, dass "Z" und "t" Arrays sind.
* Jedes Element in der Matrix "Z" ist entweder eine Ganzzahl oder eine Zeichenfolge zurück.
* "t" ist ein Array von Zeichenfolgen.
* Jede Eigenschaft ist implizit optional und alle Array möglicherweise leer.

##### <a name="schema-model"></a>Schemamodell

Die Syntax der zurückgegebenen Schema lautet:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Sie sind eine Teilmenge mit Schreibmaschine Typ Anmerkungen, codierte als dynamische Wert entspricht. Mit Schreibmaschine wäre das Beispielschema:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>zählen

    count([ Predicate ])

Gibt die Anzahl der Zeilen, die für die *Prädikat* ergibt `true`. Wenn keine- *Prädikat* angegeben ist, gibt die Gesamtzahl der Datensätze in der Gruppe. 

**Perf Tipp**: Verwenden Sie `summarize count(filter)` anstelle von`where filter | summarize count()`

> [AZURE.NOTE] Vermeiden Sie count() zum Ermitteln der Anzahl von Besprechungsanfragen, Ausnahmen oder andere Ereignisse, die aufgetreten sind. Bei [Stichproben](app-insights-sampling.md) in Betrieb genommen wurde, werden die Anzahl von Datenpunkten, die in der Anwendung Einsichten beibehalten kleiner als die Anzahl der ursprünglichen Ereignisse. Verwenden Sie stattdessen `summarize sum(itemCount)...`. Die ItemCount-Eigenschaft zeigt die Anzahl der ursprünglichen Ereignisse, die durch die einzelnen beibehalten Datenpunkte dargestellt werden.

### <a name="countif"></a>ZÄHLENWENN

    countif(Predicate)

Gibt die Anzahl der Zeilen, die für die *Prädikat* ergibt `true`.

**Perf Tipp**: Verwenden Sie `summarize countif(filter)` anstelle von`where filter | summarize count()`

> [AZURE.NOTE] Vermeiden Sie countif() zum Ermitteln der Anzahl von Besprechungsanfragen, Ausnahmen oder andere Ereignisse, die aufgetreten sind. Wenn bei [Stichproben](app-insights-sampling.md) ist, werden die Anzahl von Datenpunkten kleiner als die Anzahl der tatsächlichen Ereignisse. Verwenden Sie stattdessen `summarize sum(itemCount)...`. Die ItemCount-Eigenschaft zeigt die Anzahl der ursprünglichen Ereignisse, die durch die einzelnen beibehalten Datenpunkte dargestellt werden.

### <a name="dcount"></a>DBANZAHL

    dcount( Expression [ ,  Accuracy ])

Gibt eine Schätzung der Anzahl eindeutiger Werte von *Expr* in der Gruppe an. (Um die eindeutigen Werte aufzulisten, verwenden Sie [`makeset`](#makeset).)

*Genauigkeit*, falls angegeben, steuert das Verhältnis zwischen Geschwindigkeit und Genauigkeit.

 * `0`= die Berechnung schnellste und mindestens genaue an.
 * `1`Die Standardeinstellung, die Genauigkeit und die Berechnung verteilt; Informationen zu 0.8 % zurück.
 * `2`= am häufigsten präzise und langsamste Berechnung; Informationen zu 0,4 % zurück.

**Beispiel**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Gibt eine Schätzung der Anzahl eindeutiger Werte von *Expr* von Zeilen in der Gruppe für die *Prädikat* erfüllt ist. (Um die eindeutigen Werte aufzulisten, verwenden Sie [`makeset`](#makeset).)

*Genauigkeit*, falls angegeben, steuert das Verhältnis zwischen Geschwindigkeit und Genauigkeit.

 * `0`= die Berechnung schnellste und mindestens genaue an.
 * `1`Die Standardeinstellung, die Genauigkeit und die Berechnung verteilt; Informationen zu 0.8 % zurück.
 * `2`= am häufigsten präzise und langsamste Berechnung; Informationen zu 0,4 % zurück.

**Beispiel**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>MakeList

    makelist(Expr [ ,  MaxListSize ] )

Gibt eine `dynamic` (JSON) Array aller Werte von *Ausdruck* in der Gruppe. 

* *MaxListSize* ist eine optionale Ganzzahl Grenzwert auf die maximale Anzahl von Elementen, die zurückgegeben (der Standardwert liegt *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Gibt eine `dynamic` (JSON) Array über die Sammlung distinct-Werte, die *Ausdruck* in der Gruppe zu gelangen. (Tipp: Wenn Sie um nur die eindeutigen Werte zu zählen, verwenden Sie [`dcount`](#dcount).)
  
*  *MaxSetSize* ist eine optionale Ganzzahl Grenzwert auf die maximale Anzahl von Elementen, die zurückgegeben (der Standardwert liegt *128*).

**Beispiel**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Siehe auch die [ `mvexpand` Operator](#mvexpand-operator) für die gegenüberliegende-Funktion.


### <a name="max-min"></a>Max, min

    max(Expr)

Berechnet die maximale Anzahl von *Ausdruck*.
    
    min(Expr)

Berechnet den Minimalwert der *Ausdruck*.

**Tipp**: Dies ermöglicht es Ihnen die min oder Max auf einem eigenen – beispielsweise, die höchsten oder niedrigsten Preis. Aber wenn Sie möchten, dass andere Spalten in der Zeile – beispielsweise den Namen des Lieferanten mit den niedrigsten Preis - [Argmin oder Argmax](#argmin-argmax)verwenden.


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>Quantil, Quantile zwischen, Percentilew, percentilesw

    percentile(Expression, Percentile)

Gibt eine Schätzung für den *Ausdruck* , der das angegebene Quantil in der Gruppe an. Die Genauigkeit hängt von der Dichtefunktion der Population in der Region, der das Quantil ab.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Wie `percentile()`, aber eine Anzahl von perzentilwerten (also schneller als jede Quantil einzeln berechnen) berechnet.

    percentilew(Expression, WeightExpression, Percentile)

Gewichteten Quantil. Verwenden Sie diese Option für vorab aggregierte Daten.  `WeightExpression`ist eine ganze Zahl, die angibt, wie viele ursprüngliche Zeilen durch jede Zeile zusammengefasster dargestellt werden.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Wie `percentilew()`, aber eine Anzahl von perzentilwerten berechnet.

**Beispiele**


Der Wert der `duration` , die größer als 95 % der Stichprobe festlegen und kleiner als 5 % der Stichprobe zurück, die für die einzelnen Anforderungsnamen berechnet wird:

    request 
  	| summarize percentile(duration, 95)
      by name

Auslassen "durch..." für die gesamte Tabelle zu berechnen.

Mehrere Quantile zwischen für andere Anforderung Namen gleichzeitig zu berechnen:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Die Ergebnisse, die für die Anforderung /Events/Index anzuzeigen, 5 % der Anfragen auf in weniger als 2.44s, um diese 3.52s, geantwortet werden und 5 % sind langsamer als 6.85s.

Berechnen von mehreren Statistiken:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Gewichteten Quantile zwischen

Verwenden Sie die gewichteten Quantil-Funktionen in Fällen, in denen die Daten vorab aggregiert wurde. 

Nehmen Sie beispielsweise an Ihre app führt viele Tausende von Vorgängen pro Sekunde und deren Wartezeit wissen möchten. Einfache Lösung wäre eine Anwendung Einsichten Anfrage oder eine benutzerdefinierte Ereignis für jeden Vorgang generieren. Obwohl adaptive werden, sie zu reduzieren wirksam würden, würde viele Datenverkehr, erstellen. Sie jedoch eine noch bessere Lösung implementieren: Schreiben Sie einige Code in Ihrer app, die Daten zu aggregieren, bevor Sie es an eine andere Anwendung Einsichten zu senden. Zusammengefasste Zusammenfassung werden in regelmäßigen Abständen, verringern der Rate vielleicht an ein paar Punkte pro Minute gesendet.

Der Code erhält einen Stream von Wartezeit Maße in Millisekunden an. Beispiel:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Zählt die Maße für die in den folgenden Papierkörben:`{ 10, 20, 30, 40, 50, 100 }`

Regelmäßig, können sie eine Reihe von TrackEvent aufrufen, eine für jede Zelle, mit der benutzerdefinierte Maße in jeder Anruf an: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

In Analytics sehen Sie eine solche Gruppe von Ereignissen wie folgt aus:

`opCount` | `latency`| Bedeutung
---|---|---
8 | 10 | = 8 Vorgänge in der Klasse 10 ms
6 | 20 | = 6 Vorgänge in der Klasse 20 ms
3 | 30 | = 3 Vorgänge in der Klasse 30ms
1 | 40 | = 1 Vorgänge in der Klasse 40ms

Einen genauen Überblick über die ursprüngliche Verteilung der Ereignis Wartezeiten wir verwenden, um `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Die Ergebnisse sind gleich, als hätten wir nur verwendet `percentiles` auf dem ursprünglichen Satz von Maße.

> [AZURE.NOTE] Gewichtete Quantile zwischen gelten nicht für die [Stichprobe Daten](app-insights-sampling.md), wobei jede Zeile aufgenommene zufällige zu eine Stichprobe der ursprünglichen Zeilen statt einer Papierkorb darstellt. Die einfarbigen Quantil Funktionen stehen für gesammelten Daten geeignet.

#### <a name="estimation-error-in-percentiles"></a>Abschätzung Fehlers in Quantile zwischen

Das Aggregat Quantile zwischen bietet einen ungefähren Wert [T-Übersicht](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf)verwenden. 

Die folgenden wichtigen Punkte: 

* Die Grenzen Abschätzung Fehler, mit dem Wert, der die angeforderten Quantil variieren. Die beste Genauigkeit wird an den Enden von 0 bis [100] skalieren, Quantile zwischen 0 und 100 werden die genauen Mindest- und der Verteilung. Die Genauigkeit wird allmählich in die Mitte der Skala verringert. Es ist bei den Median ungünstigsten und bei 1 % Obergrenze ist. 
* Klicken Sie auf den Rang, die sich nicht auf den Wert werden Fehler Grenzen beobachtet. Nehmen Sie an Quantil (X 50) XML Wert zurückgegeben. Abschätzung wird sichergestellt, dass mindestens 49 % und höchstens 51 % der Werte von X kleiner als XML sind. Es gibt keine theoretische Beschränkung der Unterschied zwischen XML und tatsächliche Median Wert von X.

### <a name="stdev"></a>STABW

     stdev(Expr)

Gibt die Standardabweichung der *Ausdruck* über die Gruppe an.

### <a name="variance"></a>Varianz

    variance(Expr)

Gibt die Varianz von *Expr* über die Gruppe.

### <a name="sum"></a>Summe

    sum(Expr)

Gibt die Summe der *Ausdruck* über die Gruppe an.                      


## <a name="scalars"></a>Skalare

[Wandelt](#casts) | [Vergleiche](#scalar-comparisons)
<br/>
[GetType](#gettype) | [Hash](#hash) | [iff](#iff) |  [Isnull](#isnull) | [Isnotnull](#isnotnull) | [Notnull](#notnull) | [Toscalar](#toscalar)

Die unterstützten Typen sind:

| Typ      | Weitere Namen   | Gleichwertigen .NET Typ |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Wandelt

Sie können von einem Typ in einen anderen umwandeln. Im Allgemeinen, wenn die Konvertierung sinnvoll ist, funktioniert:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Überprüfen Sie, ob eine Zeichenfolge in einen bestimmten Typ konvertiert werden kann:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skalaren Vergleiche

||
---|---
`<` |Weniger
`<=`|Kleiner oder gleich
`>` |Größer
`>=`|Größer oder gleich
`<>`|Ist nicht gleich
`!=`|Ist nicht gleich 
`in`| Rechte Operand ist (dynamische) Arrays und linke Operand eines seiner Elemente gleich ist.
`!in`| Rechte Operand ist (dynamische) Arrays und linke Operand weist keine übergeordneten Elemente der gleich.




### <a name="gettype"></a>GetType

**Gibt**

Eine Zeichenfolge, die den zugrunde liegenden Speichertyp der einzelnen Argumente darstellt. Dies ist besonders hilfreich, wenn Sie die Werte der Art haben `dynamic`: in diesem Fall `gettype()` informiert, wie ein Wert codiert werden.

**Beispiele**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>Hash

**Syntax**

    hash(source [, mod])

**Argumente auf:**

* *Datenquelle*: die Quelle skalaren rechnet der Hash auf.
* *Rest*: die modulo Wert auf die Hashergebnis angewendet werden soll.

**Gibt**

Der Xxhash (Long) Wert der angegebenen Skalarwert, modulo den Rest angegebenen Wert (sofern angegeben).

**Beispiele**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

Die `iff()` Funktion wertet das erste Argument (das Prädikat), und gibt entweder den Wert der zweiten oder dritten Argumente je nachdem, ob das Prädikat `true` oder `false`. Das zweite und dritte Argument muss vom gleichen Typ.

**Syntax**

    iff(predicate, ifTrue, ifFalse)


**Argumente auf:**

* *Prädikat:* Ein Ausdruck, der ausgewertet wird eine `boolean` Wert.
* *IfTrue:* Ein Ausdruck, der ausgewertet wird und deren Wert zurückgegeben von der Funktion Wenn *Prädikat* ergibt `true`.
* *IfFalse:* Ein Ausdruck, der ausgewertet wird und deren Wert zurückgegeben von der Funktion Wenn *Prädikat* ergibt `false`.

**Gibt**

Diese Funktion gibt den Wert von *IfTrue* aus, wenn *Prädikat* ergibt `true`, oder den Wert der *IfFalse* andernfalls.

**Beispiel**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, Isnotnull, NotNull.

    isnull(parsejson("")) == true

Erhält ein einzelnes Argument und darüber informiert, ob er null ist.

**Syntax**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Gibt**

WAHR oder falsch, je nachdem, ob ist der Wert null oder nicht null.


|x|IsNull(x)
|---|---
| "" | falsch
|"X" | falsch
|parseJSON("")|WAHR
|parseJSON("[]")|falsch
|parseJSON("{}")|falsch

**Beispiel**

    T | where isnotnull(PossiblyNull) | count

Beachten Sie, dass andere Methoden, um diesen Effekt zu erreichen:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Wertet eine Abfrage oder einen Ausdruck aus, und gibt das Ergebnis als einzelner Wert zurück. Diese Funktion eignet sich für bereitgestellte Berechnungen. Angenommen, eine gesamte Anzahl von Ereignissen und durch anschließendes verwenden, die einen Basisplan zu berechnen.

**Syntax**

    toscalar(query)
    toscalar(scalar)

**Gibt**

Das ausgewerteten Argument. Wenn das Argument eine Tabelle ist, gibt die erste Spalte der ersten Zeile. (Empfiehlt sich das ist zum Anordnen, dass das Argument nur eine Spalte und Zeile hat.)

**Beispiel**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Boolesche literalen

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Boolesche Operatoren

    and 
    or 

    

## <a name="numbers"></a>Zahlen

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Numerischen Literalen

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Arithmetische Operatoren

|| |
|---|-------------|
| + | Hinzufügen         |
| - | Subtrahieren von    |
| * | Multiplizieren    |
| / | Dividieren      |
| % | Modulo      |
||
|`<` |Weniger
|`<=`|Kleiner oder gleich
|`>` |Größer
|`>=`|Größer oder gleich
|`<>`|Ist nicht gleich
|`!=`|Ist nicht gleich 


### <a name="abs"></a>Abs

**Syntax**

    abs(x)

**Argumente auf:**

* X - eine ganze Zahl, Real oder timespan

**Gibt**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>Papierkorb, Untergrenze

Rundet die Werte auf ganze Vielfache von einem beliebigen Lagerplatz Größe. Sehr verwendet, der [`summarize by`](#summarize-operator) Abfrage. Wenn Sie einen vielerorts Satz von Werten verfügen, werden sie in eine kleinere Menge spezifischen Werten gruppiert werden.

Alias `floor`.

**Syntax**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumente auf:**

* *Wert:* Eine Zahl, Datum oder Timespan. 
* *RoundTo:* Die "Papierkorb Größe". Eine Zahl, Datum oder Timespan, durch die dividiert *Wert*. 

**Gibt**

Das nächste Vielfache von *RoundTo* unter *Wert*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Beispiele**

Ausdruck | Ergebnis
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Der folgende Ausdruck berechnet ein Histogramm der Dauer, mit einer Zelle Größe von 1 Sekunde:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>EXP

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>Untergrenze

Ein Alias für [`bin()`](#bin).

### <a name="gamma"></a>Gamma

Die [Gammafunktion](https://en.wikipedia.org/wiki/Gamma_function)

**Syntax**

    gamma(x)

**Argumente auf:**

* *x:* Eine reelle Zahl

Für positive ganze Zahlen `gamma(x) == (x-1)!` z. B. `gamma(5) == 4 * 3 * 2 * 1`.

Siehe auch [Loggamma](#loggamma).


### <a name="log"></a>log

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`Sie sollten eine reelle Zahl > 0. Andernfalls wird Null zurückgegeben.

### <a name="loggamma"></a>loggamma


Den natürlichen Logarithmus der Absolute Wert der [Gammafunktion](#gamma).

**Syntax**

    loggamma(x)

**Argumente auf:**

* *x:* Eine reelle Zahl


### <a name="rand"></a>Zufallszahl

Einen Zufallszahlen-Generator.

* `rand()`-eine reelle Zahl zwischen 0,0 und 1,0
* `rand(n)`– eine ganze Zahl zwischen 0 und n-1




### <a name="sqrt"></a>Wurzel

Die Funktion Wurzel aus.  

**Syntax**

    sqrt(x)

**Argumente auf:**

* *x:* Eine reelle Zahl > = 0.

**Gibt**

* Eine positive Zahl so, dass`sqrt(x) * sqrt(x) == x`
* `null`Wenn das Argument negativ ist oder nicht in konvertiert werden eine `real` Wert. 




### <a name="toint"></a>ToInt

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>ToDouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Datum und Uhrzeit


[vor](#ago) | [Dayofmonth](#dayofmonth) | [Wochentag](#dayofweek) |  [Dayofyear](#dayofyear) |[Datepart](#datepart) | [Endofday](#endofday) | [Endofmonth](#endofmonth) | [Endofweek](#endofweek) | [Endofyear](#endofyear) | [Getmonth](#getmonth)|  [Getyear](#getyear) | [jetzt](#now) | [Startofday](#startofday) | [Startofmonth](#startofmonth) | [Startofweek](#startofweek) | [Startofyear](#startofyear) | [Todatetime](#todatetime) | [Totimespan](#totimespan) | [Weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Datum und Uhrzeit literalen

|||
---|---
**"DateTime"**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Wie oft werden immer in UTC. Das Datum auslassen bietet heute nacheinander.
`now()`|Die aktuelle Uhrzeit.
`now(`-*TimeSpan*`)`|`now()-`*TimeSpan*
`ago(`*TimeSpan*`)`|`now()-`*TimeSpan*
**TimeSpan**|
`2d`|2 Tage
`1.5h`|1,5 Stunde 
`30m`|30 Minuten
`10s`|10 Sekunden
`0.1s`|0,1 Sekunden
`100ms`| 100 Millisekunden
`10microsecond`|
`1tick`|100-NS
`time("15 seconds")`|
`time("2")`| 2 Tage
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Datum und Uhrzeit Ausdrücke

Ausdruck |Ergebnis
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Weniger
`<=`|Kleiner oder gleich
`>` |Größer
`>=`|Größer oder gleich
`<>`|Ist nicht gleich
`!=`|Ist nicht gleich 




### <a name="ago"></a>vor

Subtrahiert die angegebene Zeitspanne die Uhrzeit des aktuellen UTC an. Wie `now()`, diese Funktion kann in einer Anweisung mehrmals verwendet werden, und die UTC-Systemzeit auf die verwiesen wird, wird für alle Instanziierungen identisch sein.

**Syntax**

    ago(a_timespan)

**Argumente auf:**

* *A_timespan*: Intervall die Uhrzeit des aktuellen UTC subtrahiert werden soll (`now()`).

**Gibt**

    now() - a_timespan

**Beispiel**

Alle Zeilen mit einem Zeitstempel in der letzten Stunde:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Extrahiert einen angegebenen Teil eines Datums als ganze Zahl an.

**Syntax**

    datepart(part, datetime)

**Argumente auf:**

* `part:String`-{"Year", "Monat", "Tag", "Stunde", "Minute", "Sekunde", "Millisekunden", "Mikrosekunde", "Nanosekunden"}
* `datetime`

**Gibt**

Long Integer-Wert des angegebenen Teils.


### <a name="dayofmonth"></a>DayOfMonth

    dayofmonth(datetime("2016-05-15")) == 15 

Englische Anzahl der Tag des Monats.

**Syntax**

    dayofmonth(a_date)

**Argumente auf:**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>Wochentag

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Die ganze Zahl von Tagen seit der vorangehenden Sonntag, als eine `timespan`.

**Syntax**

    dayofweek(a_date)

**Argumente auf:**

* `a_date`: A `datetime`.

**Gibt**

Die `timespan` seit Mitternacht am Anfang der vorhergehenden Sonntag, eine ganze Zahl von Tagen abgerundet.

**Beispiele**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DayOfYear

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Englische Anzahl der Tag des Jahres.

**Syntax**

    dayofyear(a_date)

**Argumente auf:**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>Endofday, Endofweek, Endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getMonth

Rufen Sie die Monatsnummer (1 bis 12) in einen datetime-Wert.

**Beispiel**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getYear

Rufen Sie das Jahr in einen datetime-Wert.

**Beispiel**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>jetzt

    now()
    now(-2d)

Die aktuelle Systemzeit UTC, offset optional durch den angegebenen Zeitspanne. Diese Funktion kann in einer Anweisung mehrmals verwendet werden, und die Systemzeit auf die verwiesen wird, wird für alle Instanzen identisch sein.

**Syntax**

    now([offset])

**Argumente auf:**

* *Offset:* A `timespan`, auf die aktuelle Systemzeit UTC hinzugefügt. Standard: 0.

**Gibt**

Die aktuelle Systemzeit UTC als eine `datetime`.

    now() + offset

**Beispiel**

Bestimmt das Intervall seit dem Ereignis vom Prädikat identifiziert:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>Startofday, Startofweek, Startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Überprüfen Sie, ob eine Zeichenfolge ein gültiges Datum ist:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>WeekOfYear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Das Ergebnis ganze Zahl darstellt, die Wochennummer nach der ISO-8601 standard. Erste Wochentag ist Sonntag und die erste Woche des Jahres der Woche, die das Jahr ersten Donnerstag enthält. (Der letzten Tage eines Jahres können daher einige der die Tage der Woche 1 des nächsten Jahres enthalten, oder die ersten Tagen können einige der Woche 52 oder 53 des Vorjahrs enthalten.)


## <a name="string"></a>Zeichenfolge

[Countof](#countof) | [extrahieren](#extract) | [Extractjson](#extractjson)  | [Isempty](#isempty) | [Isnotempty](#isnotempty) | [Notempty](#notempty) | [Parseurl](#parseurl) | [Ersetzen](#replace) | [Teilen](#split) | [Strcat](#strcat) | [Strlen](#strlen) | [Teilzeichenfolge](#substring) | [Tolower](#tolower) | [Tostring](#tostring) | [Toupper](#toupper)


### <a name="string-literals"></a>Zeichenfolgenliteralen

Die Regeln sind dieselben wie in JavaScript.

Zeichenfolgen können entweder einzeln oder in doppelte Anführungszeichen eingeschlossen werden. 

Umgekehrter Schrägstrich (`\`) wird verwendet, um die escape-Zeichen wie `\t` (Registerkarte) `\n` (neue Zeile) und Instanzen des umgebenden Anführungszeichen Zeichen.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Verborgene Zeichenfolgenliteralen

Verborgene Zeichenfolgenliteralen sind Zeichenfolgen, die Analytics verdeckt werden, wenn Sie die Zeichenfolge (beispielsweise beim Verfolgen von) eingeschlossen. Verbergen alle verborgene Zeichen ersetzt durch ein Start (`*`) Zeichen.

Klicken Sie zum Erstellen eine verborgene literalen Zeichenfolge vorangestellt `h` oder 'H'. Beispiel:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Vergleichen von Zeichenfolgen

Operator|Beschreibung|Groß-/Kleinschreibung|True-Beispiel
---|---|---|---
`==`|Gleich |Ja| `"aBc" == "aBc"`
`<>` `!=`|Ist nicht gleich|Ja| `"abc" <> "ABC"`
`=~`|Gleich |Nein| `"abc" =~ "ABC"`
`!~`|Ist nicht gleich |Nein| `"aBc" !~ "xyz"`
`has`|Rechte Seite (rechte Seite) ist eine gesamte Ausdruck in der linken Seite (LHS)|Nein| `"North America" has "america"`
`!has`|Rechte Seite ist keinen vollständigen Ausdruck in LHS|Nein|`"North America" !has "amer"` 
`hasprefix`|Rechte Seite ist ein Präfix für einen Ausdruck in LHS|Nein|`"North America" hasprefix "ame"`
`!hasprefix`|Rechte Seite ist kein Präfix von einem beliebigen Begriff in LHS|Nein|`"North America" !hasprefix "mer"`
`hassuffix`|Rechte Seite ist ein Suffix eines Ausdrucks in LHS|Nein|`"North America" hassuffix "rth"`
`!hassuffix`|Rechte Seite ist kein Suffix von einem beliebigen Begriff in LHS|Nein|`"North America" !hassuffix "mer"`
`contains` | Rechte Seite tritt als Teilzeichenfolge des LHS|Nein| `"FabriKam" contains "BRik"`
`!contains`| Rechte Seite tritt nicht in LHS|Nein| `"Fabrikam" !contains "xyz"`
`containscs` | Rechte Seite tritt als Teilzeichenfolge des LHS|Ja| `"FabriKam" contains "Kam"`
`!containscs`| Rechte Seite tritt nicht in LHS|Ja| `"Fabrikam" !contains "Kam"`
`startswith`|Rechte Seite ist eine ursprüngliche Teilzeichenfolge des LHS.|Nein|`"Fabrikam" startswith "fab"`
`!startswith`|Rechte Seite ist keiner anfänglichen Teilzeichenfolge des LHS.|Nein|`"Fabrikam" !startswith "abr"`
`endswith`|Rechte Seite ist eine terminal Teilzeichenfolge des LHS.|Nein|`"Fabrikam" endswith "kam"`
`!endswith`|Rechte Seite ist keiner terminal Teilzeichenfolge LHS.|Nein|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS enthält keine Übereinstimmung für die rechte Seite|Ja| `"Fabrikam" matches regex "b.*k"`
`in`|Eines der Elemente gleich|Ja|`"abc" in ("123", "345", "abc")`
`!in`|Eines der Elemente nicht gleich|Ja|`"bc" !in ("123", "345", "abc")`

Verwenden Sie `has` oder `in` , wenn Sie das Vorhandensein eines gesamten lexikalischen Ausdrucks - testen, d. h., ein Symbol oder eine alphanumerische Word durch begrenzt nicht alphanumerische Zeichen oder Anfang oder Ende des Felds. `has`führt schneller als `contains`, `startswith` oder `endswith`. Die erste dieser Abfragen schneller ausgeführt:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Zählt Vorkommen einer Teilzeichenfolge in einer Zeichenfolge an. Einfache Zeichenfolge entspricht überlappen möglicherweise; Dies gilt nicht Regex Übereinstimmungen hervorgehoben.

**Syntax**

    countof(text, search [, kind])

**Argumente auf:**

* *Text:* Eine Zeichenfolge.
* *Suchen:* Die einfache Zeichenfolge oder reguläre Ausdruck, der in *Text*entsprechen.
* *Art:* `"normal"|"regex"` Standard `normal`. 

**Gibt**

Die Anzahl die zu durchsuchenden Zeichenfolge im Container abgeglichen werden kann. Einfache Zeichenfolge entspricht überlappen möglicherweise; Dies gilt nicht Regex Übereinstimmungen hervorgehoben.

**Beispiele**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (nicht 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>Extrahieren

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Erhalten Sie eine Übereinstimmung für einen [regulären Ausdruck](#regular-expressions) aus einer Textzeichenfolge zurück. Optional, wandelt sie dann die extrahierte Teilzeichenfolge auf den angegebenen Typ.

**Syntax**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumente auf:**

* *Regex:* Eine [reguläre Ausdrücke](#regular-expressions).
* *CaptureGroup:* Eine Positive `int` Konstante, die angibt, der Erfassungsgruppe, um zu extrahieren. 0 steht für die gesamte Übereinstimmung, 1 für den Wert von der ersten '(' Klammer')' im regulären Ausdruck, 2 oder mehr für nachfolgende Klammern gefunden.
* *Text:* A `string` zu suchen.
* *TypeLiteral:* Ein optionaler Typ Literal (z. B. `typeof(long)`). Wenn angegeben, wird die extrahierte Teilzeichenfolge in dieses Typs konvertiert. 

**Gibt**

Wenn *Regex* *Text*eine Übereinstimmung findet: die Teilzeichenfolge gegen angegebene erfassen Gruppe *CaptureGroup*ist optional in *TypeLiteral*konvertiert.

Wenn keine Übereinstimmung vorhanden ist, oder die Konvertierung fehlschlägt: `null`. 

**Beispiele**

Die Beispielzeichenfolge `Trace` wird eine Definition für durchsucht `Duration`. Die Übereinstimmung wird in konvertiert `real`, und es durch eine Zeitkonstante multipliziert (`1s`), sodass die `Duration` ist vom Typ `timespan`. In diesem Beispiel ist es gleich 123.45 Sekunden:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

In diesem Beispiel entspricht dem `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty, Isnotempty, notempty

    isempty("") == true

True, wenn das Argument eine leere Zeichenfolge ist oder null ist.
Siehe auch [Isnull](#isnull).


**Syntax**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Gibt**

Gibt an, ob das Argument Isnull oder eine leere Zeichenfolge ist.

|x|IsEmpty(x)
|---|---
| "" | WAHR
|"X" | falsch
|parseJSON("")|WAHR
|parseJSON("[]")|falsch
|parseJSON("{}")|falsch


**Beispiel**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>ParseURL

Eine URL in die Teile aufteilen.

**Syntax**

    parseurl(urlstring)

**Argumente auf:**

* *Urlstring:* EINE URL.

**Gibt**

Ein Objekt enthält die Teile als Zeichenfolgen.

**Beispiel**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>Ersetzen

Ersetzen Sie alle Regex Treffer durch eine andere Zeichenfolge ein.

**Syntax**

    replace(regex, rewrite, text)

**Argumente auf:**

* *Regex:* Der [reguläre Ausdrücke](https://github.com/google/re2/wiki/Syntax) um *Text*zu durchsuchen. Sie können erfassen Gruppen "('Klammern')" enthalten. 
* *Schreiben:* Dem Ersatz regulären Ausdruck für eine Übereinstimmung durch *MatchingRegex*vorgenommen. Verwenden Sie `\0` auf die gesamte Übereinstimmung verweisen `\1` für die erste Erfassungsgruppe, `\2` usw. für nachfolgende erfassen Gruppen.
* *Text:* Eine Zeichenfolge.

**Gibt**

*Text* nach ersetzen alle Übereinstimmungen der *Regex* mit Bewertung von *neu zu schreiben*. Treffer überlappen nicht.

**Beispiel**

Diese Anweisung:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Weist die folgenden Ergebnisse:

| x    | Str | ersetzt|
|---|---|---|
| 1    | Belegt 1.000000  | Zahl wurde: 1.000000|
| 2    | Belegt 2.000000  | Zahl wurde: 2.000000|
| 3    | Belegt 3.000000  | Zahl wurde: 3.000000|
| 4    | Belegt 4.000000  | Zahl wurde: 4.000000|
| 5    | Belegt 5.000000  | Zahl wurde: 5.000000|
 



### <a name="split"></a>Teilen

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Teilt eine angegebene Zeichenfolge entsprechend der angegebenen Trennzeichen, und gibt eine Zeichenfolge Matrix mit den enthaltenen untergeordneten Zeichenfolgen. Optional können Sie eine bestimmte Teilzeichenfolge sein kann zurückgegeben, wenn vorhanden ist.

**Syntax**

    split(source, delimiter [, requestedIndex])

**Argumente auf:**

* *Datenquelle*: die Quellzeichenfolge, die entsprechend der angegebenen Trennzeichen Zielbereich für geteilte werden sollen.
* *Trennzeichen*: das Trennzeichen, das verwendet wird, um die Quellzeichenfolge zu teilen.
* *RequestedIndex*: eine optionale nullbasiert `int`. Wenn bereitgestellt, enthalten die zurückgegebene Zeichenfolge Matrix die angeforderte Teilzeichenfolge wenn vorhanden ist. 

**Gibt**

Ein Array von Zeichenfolgen enthält, die die untergeordneten Zeichenfolgen der angegebenen Quellzeichenfolge, die durch die angegebenen Trennzeichen getrennt werden.

**Beispiele**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Verkettet zwischen 1 und 16 Argumente Zeichenfolgen werden müssen.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Länge einer Zeichenfolge.

### <a name="substring"></a>Teilzeichenfolge

    substring("abcdefg", 1, 2) == "bc"

Extrahieren einer Teilzeichenfolge aus einer bestimmten Quellzeichenfolge aus einem angegebenen Index ab. Optional kann die Länge der angeforderten Teilzeichenfolge angegeben werden muss.

**Syntax**

    substring(source, startingIndex [, length])

**Argumente auf:**

* *Quelle:* Der Quellzeichenfolge, der die Teilzeichenfolge entnommen.
* *StartingIndex:* Die nullbasierte Zeichenposition des angeforderten Teilzeichenfolge.
* *Länge:* Ein optionaler Parameter, der verwendet werden kann, um die gewünschte Anzahl von Zeichen in der Teilzeichenfolge anzugeben. 

**Gibt**

Eine Teilzeichenfolge aus der angegebenen Zeichenfolge. Die Teilzeichenfolge mit Zeichenposition StartingIndex (auf Null basierend) beginnt und bis zum Ende der Zeichenfolge oder Länge Zeichen angegeben.

**Beispiele**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>ToLower

    tolower("HELLO") == "hello"

Eine Zeichenfolge konvertiert in Kleinbuchstaben.

### <a name="toupper"></a>ToUpper

    toupper("hello") == "HELLO"

Konvertiert eine Zeichenfolge in Großbuchstaben.



### <a name="guids"></a>GUIDs

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Arrays, Objekte und dynamisch

[literalen](#dynamic-literals) | [Umwandlung](#casting-dynamic-objects) | [Operatoren](#operators) | [Klauseln lassen](#dynamic-objects-in-let-clauses)
<br/>
[ArrayLength](#arraylength) | [Extractjson](#extractjson) | [Parsejson](#parsejson) | [Bereich](#range) | [Treepath](#treepath) | [Todynamic](#todynamic) | [Zip](#zip)


So sieht das Ergebnis einer Abfrage in einer Anwendung Einsichten Ausnahme aus. Der Wert in `details` ist ein Array.

![](./media/app-insights-analytics-reference/310.png)

**Indizierung:** Indizieren von Arrays und Objekten wie in JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Jedoch über die `arraylength` und anderen Funktionen Analytics (nicht ".length"!)

**Umwandeln** In einigen Fällen ist es erforderlich, ein Element, das Sie von einem Objekt, extrahieren umgewandelt, da seinen Typ variieren kann. Beispielsweise `summarize...to` benötigt einen bestimmten Typ:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literalen** Zum Erstellen einer explizite Array oder Eigenschaft-Sammlung Objekt schreiben Sie es als JSON-Zeichenfolge und Cast aus:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**Mvexpand:** Verwenden Sie zum auseinander die Eigenschaften eines Objekts in separaten Zeilen herauszuziehen, Mvexpand aus:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**Treepath:** So finden Sie alle Pfade in einer komplexen Objekts

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**Buildschema:** Um das minimale Schema zu finden, das alle Werte des Ausdrucks in der Tabelle den Zutritt gewährt:

    exceptions | summarize buildschema(details)

Ergebnis:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Beachten Sie, dass `indexer` wird verwendet, um zu kennzeichnen, in dem Sie einen numerischen Index verwendet werden sollen. Für dieses Schema einige gültige Pfade möchten (vorausgesetzt werden, dass diese Indizes Beispiel Bereich aufweisen):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Matrix und Object-literalen

Verwenden Sie zum Erstellen eines dynamischen Literal `parsejson` (Alias `todynamic`) mit einer JSON Zeichenfolge Argumente:

* `parsejson('[43, 21, 65]')`– ein Array von Zahlen
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`– ein einzelner Wert vom dynamische Typ, der eine Zahl enthält
* `parsejson('"21"')`– ein einzelner Wert vom dynamische Typ mit einer Zeichenfolge

Beachten Sie, dass im Gegensatz zu JavaScript, JSON erfordert die die Verwendung von doppelten Anführungszeichen (`"`) um Zeichenfolgen. Daher ist es im Allgemeinen einfacher eine JSON-codierte Zeichenfolge mit einfachen Anführungszeichen literalen Angebot (`'`).

In diesem Beispiel erstellt einen dynamischen Wert und dann seine Felder verwendet:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dynamisches Objekt-Funktionen

|||
|---|---|
| *Wert* `in` *Matrix*| True, wenn ein Element des *Arrays* vorhanden ist, die == *Wert*<br/>`where City in ('London', 'Paris', 'Rome')`
| *Wert* `!in` *Matrix*| True, wenn kein Element des *Arrays* vorhanden ist, die == *Wert*
|[`arraylength(`Matrix`)`](#arraylength)| Wenn sie ein Array nicht NULL
|[`extractjson(`Pfad, Objekt`)`](#extractjson)|Verwendet den Pfad zum Navigieren in Objekt.
|[`parsejson(`Datenquelle`)`](#parsejson)| Wandelt eine JSON-Zeichenfolge in ein dynamisches Objekt um.
|[`range(`Schritt aus`)`](#range)| Ein Array von Werten
|[`mvexpand`listColumn](#mvexpand-operator) | Repliziert eine Zeile für jeden Wert in einer angegebenen Zelle in einer Liste an.
|[`summarize buildschema(`Spalte`)`](#buildschema) |Leitet das Schema Typ aus Spalteninhalt
|[`summarize makelist(`Spalte`)`](#makelist)| Fasst Gruppen von Zeilen und legt die Werte der Spalte in einer Matrix zurück.
|[`summarize makeset(`Spalte`)`](#makeset) | Fasst Gruppen von Zeilen und legt die Werte der Spalte in einer Matrix zurück, ohne Duplikate.

### <a name="dynamic-objects-in-let-clauses"></a>Dynamische Objekte in Klauseln lassen


[Lassen Sie Klauseln](#let-clause) Store dynamische Werte als Zeichenfolgen, damit diese beiden Klauseln entsprechen, und beide müssen die `parsejson` (oder `todynamic`) bevor Sie verwendet werden:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>ArrayLength

Die Anzahl von Elementen in einem Array dynamische.

**Syntax**

    arraylength(array)

**Argumente auf:**

* *Array:* A `dynamic` Wert.

**Gibt**

Die Anzahl der Elemente in der *Matrix*, oder `null` ist *Matrix* keiner Matrix zurück.

**Beispiele**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Erhalten Sie ein angegebenes Element aus einer JSON-Text mithilfe eines Ausdrucks Pfad ein. Konvertieren Sie optional die extrahierte Zeichenfolge in einen bestimmten Typ.


**Syntax**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Gibt**

Diese Funktion führt eine Abfrage JsonPath in der Datenquelle, das eine gültige JSON-Zeichenfolge, konvertiert optional in einen anderen Typ je nach das dritte Argument dieser Wert enthält.



**Beispiel**

Die Notation [Klammer] und Punktnotation sind gleichwertig:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Leistungstipps**

* Anwenden der Where-Klausel vor der Verwendung`extractjson()`
* Sollten Sie stattdessen die Übereinstimmung eines regulären Ausdrucks mit [extrahieren](#extract) verwenden. Dies sehr viel schneller ausgeführt werden kann, und ist wirksam, wenn die JSON aus einer Vorlage erstellt wird.
* Verwenden Sie `parsejson()` Wenn Sie mehr als einen Wert aus der JSON extrahieren müssen.
* Erwägen Sie die JSON analysiert bei Aufnahme durch den Typ der Spalte deklarieren dynamisch sein.

### <a name="json-path-expressions"></a>Pfad JSON-Ausdrücke

|||
|---|---|
|`$`|Stamm-Objekt|
|`@`|Aktuelle Objekt|
|`[0]`|Matrix tiefgestellt|
|`.`oder`[0]` | Untergeordnetes Element|

*(Aktuell nicht wir Platzhalter, Rekursionsebene, Union oder Segmente implementieren.)*




### <a name="parsejson"></a>parseJSON

Interpretiert eine `string` als [JSON-Wert](http://json.org/)) und gibt den Wert als `dynamic`. Es ist bei der Verwendung von übergeordnete `extractjson()` Wenn Sie mehr als ein Element einer zusammengesetzten JSON-Objekt extrahieren müssen.

**Syntax**

    parsejson(json)

**Argumente auf:**

* *Json:* Ein JSON-Dokument.

**Gibt**

Ein Objekt vom Typ `dynamic` durch *Json*angegeben.

**Beispiel**

Im folgenden Beispiel wenn `context_custom_metrics` ist ein `string` , sieht folgendermaßen aus: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

und dann das folgende Fragment den Wert des abruft der `duration` Slot in das Objekt, und die It ruft zwei Steckplätze `duration.value` und  `duration.min` (`118.0` und `110.0`Hilfethemas).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>Bereich

Die `range()` (Funktion) (nicht zu verwechselt werden die `range` Operator) mit einer Reihe von Werten gleichen Abständen dynamische Arrays generiert.

**Syntax**

    range(start, stop, step)

**Argumente auf:**

* *starten:* Der Wert des ersten Elements in der sich daraus ergebende Matrix. 
* *Beenden:* Der Wert des letzten Elements in der sich daraus ergebende Matrix oder den minimalen Wert, der größer ist als das letzte Element in der sich daraus ergebende Matrix und in eine ganze Zahl ist Vielfache von *Schritt* aus *Starten*.
* *Schritt:* Die Differenz zwischen zwei aufeinander folgende Elemente des Arrays.

**Beispiele**

Im folgende Beispiel gibt `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Im folgende Beispiel gibt ein Array aller Tage im Jahr 2015 halten:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Konvertiert eine Zeichenfolge in einen dynamischen Wert an.

### <a name="treepath"></a>TreePath

    treepath(dynamic_object)

Listet die Pfad-Ausdrücke, die Blättern in ein dynamisches Objekt zu identifizieren. 

**Gibt**

Ein Array von Ausdrücken Pfad.

**Beispiele**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Hinweis "[0]" zeigt an, das Vorhandensein einer Matrix zurück, aber es gibt keinen Indexes verwendet wird, indem Sie einen bestimmten Pfad.

### <a name="zip"></a>ZIP

    zip(list1, list2, ...)

Kombiniert eine Reihe von Listen in einer Liste der Tupel an.

* `list1...`: Eine Liste von Werten

**Beispiele**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Namen

Namen können bis zu 1024 Zeichen lang sein. Sie sind Groß-/Kleinschreibung beachtet und Buchstaben, Ziffern und Unterstriche enthalten möglicherweise (`_`). 

Angebot einen Namen mit ['... '] or [" ... "] zu anderen Zeichen, oder verwenden ein Schlüsselwort als einen Namen. Beispiel:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|[' Pfad\\\n\'x\''] | Verwenden Sie \ als Escapezeichen|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Keine Escapezeichen - \ Literal|
|[@"\now& dann\"] | |
|[Where] | Schlüsselwort als Namen Sprache|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


