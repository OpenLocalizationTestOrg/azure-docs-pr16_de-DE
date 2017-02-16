<properties
    pageTitle="Log Analytics suchen Verweis | Microsoft Azure"
    description="Der Log Analytics Suche Bezug beschrieben, die Sprache für die Suche und stellt die Abfragesyntax allgemeine Optionen können Sie wann nach Daten suchen und Filtern von Ausdrücken, um die Suche einzuschränken."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="log-analytics-search-reference"></a>Melden Sie sich Analytics Suche Bezug

Im folgenden Abschnitt der Bezug über die Sprache für die Suche beschreibt die allgemeine Syntax Abfrageoptionen wann können nach Daten suchen und Filtern von Ausdrücken, um die Suche einzuschränken. Darüber hinaus werden die Befehle, mit denen Sie die Daten, die abgerufen bezüglich, beschrieben.

Sie erhalten Informationen über die zurückgegebenen Felder in Suchvorgängen und die Aspekte, die Ihnen, Dill helfen-in ähnlichen Kategorien von Daten in das [Suchfeld und Vertriebsstrategie Abschnitt verweisen](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Allgemeine Abfragesyntax

Syntax:

```
filterExpression | command1 | command2 …
```

Der Filterausdruck (`filterExpression`) definiert die "Where" für die Abfrage Bedingung. Die Befehle beziehen sich auf die von der Abfrage zurückgegebenen Ergebnisse. Mehrere Befehle müssen durch das Pipezeichen (|) getrennt werden.

### <a name="general-syntax-examples"></a>Allgemeine Syntaxbeispiele

Beispiele:

```
system
```

Diese Abfrage gibt Ergebnisse, die das Wort "System" in jedes Feld, das für Sie Ausdrücke suchen oder vollständigen Text indiziert wurde.

>[AZURE.NOTE] Nicht alle Felder auf diese Weise indiziert sind, aber die am häufigsten verwendeten Textfelder (z. B. Beschreibungen und Namen) in der Regel wäre.

```
system error
```

Diese Abfrage gibt Ergebnisse, die die Wörter *System* und die *Fehler*enthalten.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Diese Abfrage gibt Ergebnisse, die die Wörter *System* und die *Fehler*enthalten. Es wird die Ergebnisse, indem Sie das Feld *"Verwaltungsgruppenname"* (in aufsteigender Reihenfolge) und dann nach *TimeGenerated* (in absteigender Reihenfolge) sortiert. Nur die ersten 10 Ergebnisse dauert.

>[AZURE.IMPORTANT] Die Feldnamen und die Werte für die Felder Zeichenfolge und Text werden Groß-/Kleinschreibung beachtet.

## <a name="filter-expression"></a>Filter-Ausdruck

Die folgenden Unterabschnitte erläutern die Filterausdrücke.

### <a name="string-literals"></a>Zeichenfolgenliteralen

Eine Zeichenfolge ist eine beliebige Zeichenfolge, die vom Parser als ein Schlüsselwort oder einen vordefinierten Datentyp (beispielsweise eine Zahl oder Datum) nicht erkannt wird.

Beispiele:

```
These all are string literals
```

Diese Abfrage sucht nach Ergebnisse, die Vorkommen des alle fünf Wörter enthalten. Um eine komplexe Zeichenfolgensuche durchführen zu können, setzen Sie die Zeichenfolge literal in Anführungszeichen ein, beispielsweise:

```
" Windows Server"
```

Dies gibt nur Ergebnisse mit genauen Treffer für "Windows Server".

### <a name="numbers"></a>Zahlen

Der Parser unterstützt die dezimale ganze Zahl und Gleitkommazahl Zahl Syntax für numerische Felder.

Beispiele:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="datetime"></a>Datum/Uhrzeit

Jedes Datenelement im System verfügt über eine *TimeGenerated* -Eigenschaft, die das Datum und die Uhrzeit des Datensatzes darstellt. Einige Arten von Daten können darüber hinaus weitere Datums-/Uhrzeitfelder (z. B. *LastModified*) sein.

Die Zeitachse Diagramm/Uhrzeit-Auswahl in Log Analytics zeigt eine Verteilung von Ergebnissen über die Zeit (gemäß der aktuellen Abfrage ausgeführt wird), basierend auf dem *TimeGenerated* -Feld. Datum/Uhrzeit-Felder haben, einem bestimmten Zeichenfolgenformat, die in Abfragen verwendet werden kann, um die Abfrage auf einer bestimmten Zeitspanne einzuschränken. Syntax können Sie auch auf relative Zeitspannen (beispielsweise "zwischen 3 Tagen und 2 Stunden zurück") verweisen.

Syntax:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Beispiel:

```
TimeGenerated:2013-10-01T12:20
```

Der vorherige Befehl gibt nur Datensätze mit dem Wert *TimeGenerated* genau 12:20 am 1. Oktober 2013. Es ist wahrscheinlich nicht, dass es werden alle Ergebnis bereitstellen, aber Sie wissen, dass die Idee.

Der Parser unterstützt jetzt auch den mnemonischen Datum/Uhrzeit-Wert.


In diesem Fall ist es wahrscheinlich nicht, dass dies alle Ergebnis Rendite werden, da die Daten, die das schnelle System werden nicht.

In diesen Beispielen werden die Bausteine für relativen und absoluten Datumsangaben verwendet werden soll. In den nächsten drei Unterabschnitte wird erläutert, wie sie in erweiterte Filter mit Beispielen verwendet, die relative Datumsbereiche verwenden.

### <a name="datetime-math"></a>Datum/Uhrzeit-Mathematik

Datum/Uhrzeit mathematische Operatoren versetzt oder Datum/Uhrzeit-Werts mithilfe von einfachen Datum/Uhrzeit-Berechnungen runden verwendet.

Syntax:

```
datetime/unit
```

```
datetime[+|-]count unit
```

|Operator|Beschreibung|
|---|---|
|/|Datum/Uhrzeit auf der angegebenen Einheit abgerundet. Beispiel: Jetzt / Tag rundet die aktuelle Datum/Uhrzeit der Mitternacht des aktuellen Tags.|
|+ oder -|Versetzt Datum/Uhrzeit, um die angegebene Anzahl von Einheiten. Beispiele: jetzt + 1 Stunde versetzt Sie die aktuelle Uhrzeit, um eine Stunde ahead.2013-10-01T12:00-10 Tagen versetzt den Datumswert von 10 Tagen zurück.|



Sie können das Datum/Uhrzeit mathematische Symbole, beispielsweise verketten:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Die folgende Tabelle enthält die unterstützten Datum/Uhrzeit-Einheiten.

Datum/Uhrzeit-Einheit|Beschreibung
---|---
JAHR, JAHRE|Rundet auf aktuelle Jahr oder versetzt um die angegebene Anzahl von Jahren.
MONAT, MONATE|Rundet zum aktuellen Monat oder versetzt um die angegebene Anzahl von Monaten zurück.
TAGE, DATUM|Rundet zum aktuellen Tag des Monats oder versetzt um die angegebene Anzahl von Tagen.
STUNDE, STUNDEN|Rundet die aktuelle Stunde oder versetzt um die angegebene Anzahl von Stunden.
MINUTE MINUTEN|Rundet auf aktuelle Minute oder versetzt um die angegebene Anzahl von Minuten.
ZWEITES, SEKUNDEN|Rundet auf aktuelle zweites oder versetzt um die angegebene Anzahl von Sekunden.
MILLISEKUNDEN, MILLISEKUNDEN, MILLI, MILLIS|Rundet zum aktuellen Millisekunden oder versetzt um die angegebene Anzahl von Millisekunden.


### <a name="field-facets"></a>Feld Aspekte

Mithilfe von Feld Aspekte, können Sie die Suche Bedingung für bestimmte Felder und deren genauen Werte, im Gegensatz zum Schreiben von "kostenlosen Text" Abfragen für verschiedene Ausdrücke in der gesamten Index angeben. Wir haben bereits diese Syntax in mehreren Beispielen in den vorherigen Abschnitten verwendet. Hier stellen wir komplexere Beispiele.

**Syntax**

```
field:value
```

```
field=value
```

**Beschreibung**

Durchsucht das Feld für den jeweiligen Wert an. Der Wert kann eine Zeichenfolgenliteral, Zahl oder Datum/Uhrzeit sein.

Beispiel:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntax**

*Feld > Wert*

*Feld < Wert*

*Feld > = Wert*

*Feld < = Wert*

*Feld! = Wert*

**Beschreibung**

Enthält Vergleiche sind möglich.

Beispiel:

```
TimeGenerated>NOW+2HOURS
```

**Syntax**

```
field:[from..to]
```

**Beschreibung**

Stellt Faceting Bereich.

Beispiel:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="logical-operators"></a>Logische Operatoren

Die Abfragesprachen unterstützen die logischen Operatoren (*und*, *oder*und *nicht*) und ihre Aliase C-Format (*&&*, *||*, und *!*) Hilfethemas. Sie können Klammern zum Gruppieren von folgenden Operatoren verwenden.

Beispiele:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Sie können den logischen Operator für den Filter der obersten Ebene Argumente auslassen. In diesem Fall wird der Operator und angenommen.


Filter-Ausdruck|Entspricht
---|---
Systemfehler|System und zurück
System "Windows Server" oder Schwere: 1|System und ("Windows Server" oder Schwere: 1)

### <a name="wildcarding"></a>Einfachen Platzhalterverwendung entsteht

Die Verwendung die Abfragesprache unterstützt die (*\*) Zeichen ein oder mehrere Zeichen für einen Wert in einer Abfrage darstellen.

Beispiele:

 Alle Computer mit "SQL" in den Namen ein, beispielsweise "Redmond-SQL" suchen.

```
Type=Event Computer=*SQL*
```

>[AZURE.NOTE] Platzhalter können heute in Anführungszeichen verwendet werden. Nachricht =`"*This text*"` berücksichtigen der (\*) als Literal verwendet (\*) Zeichen.
## <a name="commands"></a>Befehle

Die Befehle beziehen sich auf die Ergebnisse, die von der Abfrage zurückgegeben werden. Verwenden Sie das Pipezeichen (|), um einen Befehl an die abgerufenen Ergebnisse anzuwenden. Mehrere Befehle müssen durch den senkrechten Strich getrennt werden.

>[AZURE.NOTE] Namen der Befehle können in Groß- oder Kleinbuchstaben, im Gegensatz zu den Feldnamen und die Daten geschrieben werden.

### <a name="sort"></a>Sortieren

Syntax:

    sort field1 asc|desc, field2 asc|desc, …

Die Ergebnisse nach bestimmten Feldern sortiert. Das Präfix Asc/Desc ist optional. Wenn sie nicht angegeben werden, wird die Sortierreihenfolge *Asc* angenommen. Wenn eine Abfrage mit dem Befehl ' *Sortieren* ' nicht explizit verwenden, sortieren **TimeGenerated** Desc ist das Standardverhalten, und es wird immer die letzte Ergebnisse zuerst zurück.

### <a name="toplimit"></a>Oben/Grenzwert

Syntax:

    top number


    limit number
Wird die Antwort auf die Top N Ergebnisse beschränkt.

Beispiel:

    Type:Alert errors detected | top 10

Gibt die obersten 10 übereinstimmenden Ergebnisse zurück.

### <a name="skip"></a>Überspringen

Syntax:

    skip number

Überspringt die Anzahl der Ergebnisse aufgeführt.

Beispiel:

    Type:Alert errors detected | top 10 | skip 200

Gibt die obersten 10 übereinstimmenden Ergebnisse Ergebnis 200 ab.

### <a name="select"></a>Wählen Sie aus

Syntax:

    select field1, field2, ...

Beschränkt die Ergebnisse auf die Felder ausgewählt haben.

Beispiel:

    Type:Alert errors detected | select Name, Severity

Wird die zurückgegebenen Ergebnisse Felder *Name* und *schwere*beschränkt.

### <a name="measure"></a>Measure

Der Befehl *Measure* wird verwendet, um die statistische Funktionen auf den unformatierten Suchergebnissen anzuwenden. Dies ist sehr hilfreich *Group by* Ansichten über die Daten zu erhalten. Wenn Sie den Befehl *Measure* verwenden, zeigt Log Analytics suchen eine Tabelle mit aggregierten Ergebnisse aus.

Syntax:

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Die Ergebnisse mittels *GroupField* aggregiert und die aggregierte Measurewerte mithilfe von *AggregatedField*berechnet.


|Measure statistische (Funktion)|Beschreibung|
|---|---|
|*aggregateFunction*|Der Name der Aggregatfunktion (ohne Berücksichtigung von Groß-und Kleinschreibung). Die folgenden Aggregatfunktionen werden unterstützt: zählen MAX MIN Summe Mittelwert STDDEV COUNTDISTINCT Quantil ## oder PCT ## (## ist eine Zahl zwischen 1 und 99)|
|*aggregatedField*|Das Feld, das gerade zusammengesetzt wird. Dieses Feld ist optional für die COUNT-Aggregatfunktion, besitzt aber zu einem vorhandenen numerischen Feld Summe, MAX, MIN, AVG STDDEV, oder Quantil ## oder PCT ## liegen (## ist eine Zahl zwischen 1 und 99). Die AggregatedField kann auch den Erweiterungsmodus unterstützte Funktionen darstellen.|
|*fieldAlias*|(Optional) Alias für die berechnete Aggregatwert. Wenn nicht angegeben, werden der Feldnamen AggregatedValue.|
|*groupField*|Der Name des Felds, das das Resultset ist nach gruppiert.|
|*Intervall*|Das Zeitintervall im Format:**NnnNAME** Where: Nnn wird der positive ganze Zahl. **Ist der Name des Intervalls.** Unterstützte Intervall Namen enthalten (Groß-/Kleinschreibung wird beachtet): MILLISEKUNDEN [S] zweiten [S] MINUTE [S] Stunde [S] Tag [S] Monat [S] Jahr [S]|


Die Intervalloption kann nur in Datum/Uhrzeit-Gruppenfelder (z. B. *TimeGenerated* und *TimeCreated*) verwendet werden. Aktuell, dies nicht vom Dienst erzwungen, aber ein Feld ohne Datum/Uhrzeit, die an die Back-End-übergeben wird bewirkt, dass einen Laufzeitfehler. Bei der Prüfen des Schemas implementiert wird, lehnt die API Abfragen, die ohne Datum/Uhrzeit-Felder zur Aggregation Intervall verwenden. Die aktuelle *Measure* -Implementierung unterstützt Intervall für eine Aggregatfunktion gruppieren.

Wenn die BY-Klausel nicht angegeben wird, aber ein Intervall (als eine zweite Syntax) angegeben ist, wird das Feld *TimeGenerated* standardmäßig angenommen.

Beispiele:

**Beispiel 1**

    Type:Alert | measure count() as Count by ObjectId

*Erläuterung*

Die Warnungen *ObjectID* gruppiert und die Anzahl der Benachrichtigungen für jede Gruppe berechnet. Der Aggregatwert wird als Feld *Anzahl* (Alias) zurückgegeben.

**Beispiel 2**

    Type:Alert | measure count() interval 1HOUR

*Erläuterung*

Die Benachrichtigungen nach 1 Stunde Intervallen über das Feld *TimeGenerated* gruppiert, und gibt die Anzahl der Benachrichtigungen in den einzelnen Intervallen.

**Beispiel 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

*Erläuterung*

Identisch mit dem vorherigen Beispiel, aber mit einem aggregiertes Feldalias (*AlertsPerHour*).

**Beispiel 4**

    * | Messen Sie count() TimeCreated Intervall 5DAYS

*Erläuterung*

Die Ergebnisse von 5 Tagen Intervallen über das Feld *TimeCreated* gruppiert, und gibt die Anzahl der Ergebnisse in den einzelnen Intervallen.

**Beispiel 5**

    Type:Alert | measure max(Severity) by WorkflowName

*Erläuterung*

Die Benachrichtigungen Arbeitsbelastung namentlich gruppiert, und gibt den Wert maximale benachrichtigen schwere für jeden Workflow.

**Beispiel 6**

    Type:Alert | measure min(Severity) by WorkflowName

*Erläuterung*

Wie im vorherigen Beispiel, jedoch mit der *Min* aggregiert (Funktion).

**Beispiel 7**

    Type:Perf | measure avg(CounterValue) by Computer

*Erläuterung*

Gruppiert nach Computer Perf und berechnet den Mittelwert (Durchschnitt).

**Beispiel 8**

    Type:Perf | measure sum(CounterValue) by Computer

*Erläuterung*

Identisch mit dem vorherigen Beispiel verwendet *Summe*aber.

**Beispiel 9**

    Type:Perf | measure stddev(CounterValue) by Computer

*Erläuterung*

Identisch mit dem vorherigen Beispiel verwendet *STDDEV*aber.

**Beispiel 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

*Erläuterung*

Identisch mit dem vorherigen Beispiel verwendet *PERCENTILE70*aber.

**Beispiel 11**

    Type:Perf | measure pct70(CounterValue) by Computer

*Erläuterung*

Identisch mit dem vorherigen Beispiel verwendet *PCT70*aber. Beachten Sie, dass *PCT ##* nur ein Alias für *Quantil ##* -Funktion ist.

**Beispiel für 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

*Erläuterung*

Perf zunächst nach Computer gruppiert und dann CounterName berechnet den Mittelwert (Durchschnitt).

**Beispiel für 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

*Erläuterung*

Ruft die oberen fünf Workflows für die maximale Anzahl von Benachrichtigungen ab.

**Beispiel für 14**

    * | Messen countdistinct(Computer) nach Typ

*Erläuterung*

Zählt die Anzahl der eindeutigen reporting für jeden Computer an.

**Beispiel: 15**

    * | Messen Sie countdistinct(Computer) Intervall 1 Stunde

*Erläuterung*

Zählt die Anzahl der eindeutigen für jede Stunde reporting Computer an.

**Beispiel für 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

*Erläuterung*

% Prozessor Entstehung von Computer gruppiert, und gibt den Mittelwert für jede 1 Stunde.

**Beispiel 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

*Erläuterung*

W3CIISLog nach der Methode gruppiert, und gibt das Maximum für alle 5 Minuten.

**Beispiel für 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

*Erläuterung*

% CPU-Zeit nach Computer gruppiert, und gibt das Minimum, Mittelwert, 75 %-Quantil und Maximum für jede 1 Stunde.

**Beispiel für 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

*Erläuterung*

% CPU-Zeit erst nach Computer und dann nach Instanznamen gruppiert sind, und gibt das Minimum, Mittelwert, 75 %-Quantil und Maximum für jede Stunde

**Beispiel für 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

*Erläuterung*

Berechnet, dass die maximale Anzahl von Datenträger pro Minute für jeden Datenträger auf dem Computer schreibt

### <a name="where"></a>WHERE

Syntax:

```
**where** AggregatedValue>20
```

Kann nur nach einem Befehl *Measure* verwendet werden in der aggregierten Ergebnissen weiter zu filtern, die die Aggregatfunktion *Measure* erzeugt wurde.

Beispiele:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)

### <a name="in"></a>IN

Syntax:

```
(Outer Query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Beschreibung: Diese Syntax können Sie eine Aggregation erstellen und feed der Liste mit Werten aus dieser Aggregation in einer anderen äußeren (primär) suchen, die für Ereignisse mit diesen Wert aussehen wird. Hierzu trennen die innere Suche in geschweifte Klammern und ihre Ergebnisse als möglichen Werte für ein Feld in der äußeren suchen, die mit dem Operator IN einzuziehen.

Innere Abfrage Beispiel: mit der folgenden Abfrage für die Aggregation *Computern aktuell fehlende Sicherheitsupdates* :

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Die fertige Abfrage, die *Alle Windows-Ereignisse für Computern aktuell fehlende Sicherheitsupdates* findet aussehen würde:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="dedup"></a>Dedup

**Syntax**

    Dedup FieldName

**Beschreibung** Gibt das erste Dokument für jeden eindeutigen Wert des angegebenen Felds gefunden.

**Beispiel**

    Type=Event | sort TimeGenerated DESC | Dedup EventID

Im Beispiel oben gibt ein Ereignis (das neuste, da wir DESC auf TimeGenerated verwenden) pro EventID


### <a name="extend"></a>Erweitern

**Beschreibung** Ermöglicht das Laufzeit-Felder in Abfragen erstellen. Sie können auch Measure Befehl nach Erweiterungsmodus verwenden, wenn Aggregation ausgeführt werden soll.

**Beispiel 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Anzeigen der gewichteten Formulierung für SQL-Bewertung Empfehlungen

**Beispiel 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Anzeigen der Zählerwert in KB anstelle von Bytes

**Beispiel 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Den Wert der WireData TotalBytes skaliert werden, dass alle Ergebnisse zwischen 0 und 100 liegen.

**Beispiel 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Kategorisieren von Perf Zählerwerte kleiner als 50 % Las niedrig und andere als hoch

**Beispiel 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Berechnet, dass die maximale Anzahl von Datenträger pro Minute für jeden Datenträger auf dem Computer schreibt

**Unterstützte Funktionen**


| (Funktion) | Beschreibung | Syntaxbeispiele |
|---------|---------|---------|
| Abs | Gibt den absoluten Wert der angegebenen Wert oder der Funktion an. | `abs(x)` <br> `abs(-5)` |
| ARCCOS | Gibt einen Wert oder eine Funktion Arkuskosinus | `acos(x)` |
| und | Gibt einen Wert true zurück, wenn alle zugehörigen Operanden wahr ausgewertet werden. | `and(not(exists(popularity)),exists(price))` |
| ARCSIN | Gibt einen Wert oder eine Funktion Arkussinus | `asin(x)` |
| ARCTAN | Gibt einen Wert oder eine Funktion Arkustangens | `atan(x)` |
| ARCTAN2 |  Gibt den Winkel aus der Umwandlung von Rechteck-Koordinaten resultierende X, y, um Polarkoordinaten | `atan2(x,y)` |
| cbrt | Cube Stamm |  `cbrt(x)` |
| ceil | Rundet eine ganze Zahl zurück | `ceil(x)`  <br> `ceil(5.6)`-Gibt 6 |
| COs | Gibt Kosinus eines Winkels zurück. | `cos(x)` |
| COSHYP | Hyperbolischen Kosinus eines Winkels zurück | `cosh(x)` |
| Qualität | Qualität ist die Abkürzung für Standard. Gibt den Wert des Felds "Feld", oder wenn das Feld nicht vorhanden ist, gibt Sie den Standardwert angegeben und den ersten Wert ergibt, wo: `exists()==true`. | `def(rating,5)`– Diese def()-Funktion gibt die Bewertung, oder wenn keine Bewertung im Dokument angegeben, gibt 5 zurück <br> `def(myfield, 1.0)`-entspricht`if(exists(myfield),myfield,1.0)` |
| Grad | Konvertieren von Bogenmaß (Radiant) in Grad um |  `deg(x)` |
| div | `div(x,y)`dividiert x von y. | `div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| Verteiler | Gibt den Abstand zwischen zwei Vektoren, (Punkt) in eine n mehrdimensional Leerzeichen zurück. Rundet in die Potenz plus zwei oder mehr ValueSource Instanzen und berechnet die Abstände zwischen die zwei dar. Jede ValueSource muss eine Zahl sein. Es muss eine gerade Zahl von ValueSource Instanzen übergeben, und die Methode wird davon ausgegangen, dass die erste Hälfte den ersten Vektor und die zweite Hälfte darstellen des zweiten Vektors.  | `dist(2, x, y, 0, 0)`– Berechnet den Abstand euklidischen between,(0,0) und (X, y) für jedes Dokument. <br> `dist(1, x, y, 0, 0)`-Berechnet Manhattan (kurz), Abstand zwischen (0,0) und (x, y) für jedes Dokument. <br> `dist(2,,x,y,z,0,0,0)`-Euklidischen Abstand zwischen (0,0,0) und (X, y, Z) für jedes Dokument.<br>`dist(1,x,y,z,e,f,g)`-Manhattan Abstand zwischen (X, y, Z) und (e, f, g), in dem jeder Brief ist ein Feldname. |
| vorhanden ist | Gibt True, wenn ein Element im Feld vorhanden ist. | `exists(author)`-Gibt TRUE für jedes Dokument weist einen Wert in das Feld "Autor".<br>`exists(query(price:5.00))`– Gibt true zurück, wenn entspricht "Preis", "5,00". |
| EXP | Gibt die Eulersche Zahl potenziert mit x | `exp(x)` |
| Untergrenze | Rundet nach unten zu einer ganzen Zahl | `floor(x)`  <br> `floor(5.6)`-Gibt 5 |
| hypo | Gibt sqrt(sum(pow(x,2),pow(y,2))) ohne mittlere Überlauf oder Unterlauf | `hypo(x,y)`  <br> ` |
| if | Ermöglicht bedingte Funktion Abfragen. In `if(test,value1,value2)` -Test ist oder bezieht sich auf einen logischen Wert oder Ausdruck, der einen logischen Wert (WAHR oder falsch) zurückgibt.  `value1`ist der Wert, der von der Funktion zurückgegeben wird, wenn Test TRUE ergibt. `value2`ist der Wert, der von der Funktion zurückgegeben wird, wenn der Test FALSE ergibt. Ein Ausdruck kann jede Funktion die boolesche Werte zurückgibt, oder sogar Funktionen zurückgeben numerische Werte, die in denen Groß-/Kleinschreibung Wert 0 als falsch oder Zeichenfolgen, interpretiert wird in diesem Fall leere Zeichenfolge wird interpretiert als falsch. | `if(termfreq(cat,'electronics'),popularity,42)`– Diese Funktion überprüft jedes Dokument für das anzeigen, wenn sie den Ausdruck "Elektronik" in das Feld "Katze" enthält. Wenn dies der Fall ist, wird Klicken Sie dann der Wert des Felds Beliebtheit zurückgegeben wird, sonst der Wert von 42 zurückgegeben. |
| lineare | Implementiert `m*x+c` , wo sind m und c Konstanten und x ist eine zufällige (Funktion). Dieser Wert entspricht `sum(product(m,x),c)`, aber etwas effizienter, da es als einzelne Funktion implementiert wird. | `linear(x,m,c) linear(x,2,4)`Gibt`2*x+4` |
| LN| Der natürliche Logarithmus der angegebenen Funktion gibt |  `ln(x)` |
| log | Gibt das Protokoll der angegebenen Funktion zur Basis 10 zurück. | `log(x)   log(sum(x,100))` |
| Karte | Ordnet alle Werte einer Eingabewerte X-Funktion, die auf das angegebene Ziel innerhalb min und Max (einschließlich) liegen. Die Argumente min und Max müssen Konstanten sein. Die Argumente Ziel und Standardwert können Konstanten oder Funktionen sein. Wenn der Wert von x nicht zwischen min und Max fällt, klicken Sie dann entweder wird der Wert von x zurückgegeben, oder ein Standardwert wird zurückgegeben, wenn als ein 5. Argument angegeben. |  `map(x,min,max,target) map(x,0,0,1)`-Ändert alle Werte 0 bis 1. Dies kann sinnvoll Behandeln von Standardwerten 0 sein.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`-Ändert sich in-1 alle Werte zwischen 0 und 100, 1 und alle anderen Werte.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`-Ändert sich alle Werte zwischen 0 und 100 bis X + 599 sowie alle anderen Werte in der Häufigkeit von der Begriff 'Solr' im Feldtext ein. |
| Max | Gibt den größten numerischen Wert aus mehreren verschachtelten Funktionen oder Konstanten, die als Argumente angegeben sind: `max(x,y,...)`. Max-Funktion kann auch für "bottoming" eine andere Funktion nützlich sein oder Feld bei einigen angegebenen Konstante.  Verwenden der `field(myfield,max)` Syntax für den größten Wert aus einem einzelnen mehrwertiges Feld auswählen.  | `max(myfield,myotherfield,0)` |
| Min | Gibt den kleinsten numerischen Wert mehrere verschachtelte Funktionen Konstanten, die als Argumente angegeben sind: `min(x,y,...)`. Min-Funktion kann auch für die Bereitstellung "Obergrenze" auf eine Funktion mit einer Konstanten nützlich sein. Verwenden der `field(myfield,min)` Syntax für den kleinsten Wert aus einem einzelnen mehrwertiges Feld auswählen. | `min(myfield,myotherfield,0)` |
| Rest | Berechnet den Rest von der Funktion X von y (Funktion). |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))`   |
| MS | Gibt in Millisekunden an den Unterschied zwischen der zugehörigen Argumente zurück. Datumsangaben werden relativ zum Unix oder POSIX Zeit Epoche, dem 1. Januar 1970 UTC. Argumente möglicherweise der Namen einer indizierten TrieDateField oder Datum Mathematik basierend auf einem Konstanten Datum oder werden jetzt. `ms()`entspricht dem `ms(NOW)`, in Millisekunden seit der Epoche Zahl. `ms(a)`Gibt die Anzahl der Millisekunden seit der Epoche, die das Argument darstellt. `ms(a,b)`Gibt die Anzahl der Millisekunden an, dass b tritt auf, bevor Sie ein, welche ist `a - b`.| `ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| nicht | Der logisch negierten Wert der Umgebrochene-Funktion. | `not(exists(author))`-Nur, wenn WAHR `exists(author)` ist "false". |
| oder | Eine logische Verknüpfung. | `or(value1,value2)`-True zurück, wenn Wert1 oder Wert2 true ist. |
| Pow | Löst aus der angegebenen Basis in der angegebenen Potenz. `pow(x,y)`Löst x von y hoch. |  `pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`-Identisch Wurzel. |
| Produkt | Gibt das Produkt von mehreren Werten oder -Funktionen, die in einer durch Trennzeichen getrennte Liste angegeben werden. `mul(...)`möglicherweise auch als Alias für diese Funktion verwendet werden. | `product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip | Mit eine anderen Funktion führt `recip(x,m,a,b)` implementieren `a/(m*x+b)` , m, a, b Konstanten und x ist jede beliebige Komplexität Funktion. Wenn eine und b gleich sind, und x > = 0, gibt diese Funktion weist einen Höchstwert von 1, die als x erhöht legt ab. Erhöhen den Wert von einer und b nicht trennen Ergebnisse in einer Bewegung der gesamten Funktion zu einem flachere Teil der Kurve. Diese Eigenschaften umso dies eine ideale Funktion für weitere zuletzt verwendete Dokumente verstärken, wenn x ist `rord(datefield)`. | `recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad | Konvertieren von Grad in Bogenmaß (Radiant) | `rad(x)` |
| Drucken| Rundet auf die nächste ganze Zahl | `rint(x)`  <br> `rint(5.6)`-Gibt 6 |
| sin | Gibt Sinus eines Winkels zurück. | `sin(x)` |
| SINHYP | Gibt hyperbolischen Sinus eines Winkels zurück. | `sinh(x)` |
| Skalieren | Werte der Funktion X skaliert, so, dass sie zwischen den angegebenen MinTarget und MaxTarget (einschließlich) liegen. Die aktuelle Implementierung durchläuft alle Funktionswerte erhalten die min und Max, damit sie die richtige Skalierung auswählen kann. Die aktuelle Implementierung kann nicht unterscheiden, wenn Dokumente gelöscht wurden oder Dokumente, die keinen Wert aufweisen. In diesen Fällen verwendet 0.0 Werte. Dies bedeutet, dass wenn Werte normalerweise alle größer als 0.0 sind, eine weiterhin 0,0 als Minimalwert Zuordnen von einhandeln kann. In diesen Fällen eine geeigneten `map()` Funktion zum verwendet werden können zur Umgehung dieses Problems 0,0 auf einen Wert in den Bereich real ändern wie hier dargestellt:`scale(map(x,0,0,5),1,2)` | `scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`-Skaliert die Werte von x so, dass alle Werte zwischen 1 und 2 (einschließlich) sein soll. |
| Wurzel | Gibt die Wurzel aus der angegebenen Wert oder der Funktion an. | `sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist | Den Abstand zwischen zwei Textzeichenfolgen zu berechnen. Verwendet die Lucene Rechtschreibprüfung Designdetektiv StringDistance Schnittstelle sowie ist die Anwendung stecken Sie eigene über Solrs Ressource laden Funktionen unterstützt alle Implementierungen der in diesem Paket verfügbar. Strdist dauert `(string1, string2, distance measure)`. Mögliche Werte für Abstand Measure sind: <br>Jw: Jaro-Winkler<br>Bearbeiten: Levenstein oder bearbeiten Abstand<br>NGRAM: der NGramDistance, wenn angegeben, kann optional übergeben in der Ngram Größe zu. 2 der Standardwert liegt.<br>FQN: Vollqualifizierte Klasse Namen für eine Implementierung der Schnittstelle StringDistance. Benötigen Sie einen Konstruktor keine-Argument.|`strdist("SOLR",id,edit)` |
| Sub | Gibt die x-und y-von `sub(x,y)`. | `sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Summe | Gibt die Summe von mehreren Werten oder -Funktionen, die in einer durch Trennzeichen getrennte Liste angegeben werden. `add(...)`möglicherweise als Alias für diese Funktion verwendet werden. | `sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)`|
|termfreq | Gibt die Anzahl der Häufigkeit, mit die der Ausdruck in das Feld für dieses Dokument angezeigt wird. | termfreq(Text,'memory')|
| tan | Gibt Tangens eines Winkels zurück. | `tan(x)` |
| TANHYP | Hyperbolischen Tangens eines Winkels zurück | `tanh(x)` |



## <a name="search-field-and-facet-reference"></a>Suche Feld und Vertriebsstrategie Bezug

Wenn Sie Log-Suche verwenden, um Daten zu suchen, Anzeigen verschiedene Feld und Aspekte Ergebnisse. Jedoch möglicherweise einige Informationen, die Sie sehen nicht sehr aussagekräftig angezeigt. Die folgende Informationen können Sie die Ergebnisse zu verstehen.



|Feld|Search Type|Beschreibung|
|---|---|---|
|TenantId|Alle|Verwendet, um Daten|
|TimeGenerated|Alle|Verwendet, um die Zeitachse, Timeselectors (im Feld Suche und in anderen Bildschirmen) versorgen. Ihn darstellt, wenn die speziellen Daten (in der Regel auf der Agent) erzeugt wurde. Die Zeit wird im ISO-Format ausgedrückt und immer UTC ist. Wenn "Typen", die auf vorhandenen Instrumentation (d. h. Ereignisse in ein Protokoll) basieren handelt es sich in der Regel die Bearbeitung von Dokumenten, die an der Log Eintrag/Linie/Eintrag angemeldet war; für einige der anderen Typen, die über Management Packs oder in der Cloud – gefertigt wurden h. Empfehlungen/Benachrichtigungen/Updateagent/usw., dies die Uhrzeit, wann diese neue Datenelemente mit einer Momentaufnahme einer Konfiguration irgendeiner gesammelt wurde oder eine Empfehlungen-Warnung erzeugt wurde, daran basiert.|
|EventID|Ereignis|EventID in die Windows-Ereignisprotokolls|
|Ereignisprotokoll|Ereignis|Ereignisprotokoll, in dem das Ereignis von Windows protokolliert wurde|
|EventLevelName|Ereignis|Kritische / Warnung / Informationen / Erfolg|
|EventLevel|Ereignis|Numerische Wert für kritische / Warnung / Informationen / Erfolg (stattdessen EventLevelName für einfacher/besser lesbar Abfragen)|
|SourceSystem|Alle|Ursprung der Daten aus (im Hinblick auf den Dienst - Manager h. Vorgänge, OMS Modus 'anfügen' (= die Daten in der Cloud generiert werden), Azure-Speicher (Daten aus WAD stammen) usw.|
|Objektname|PerfHourly|Windows-Leistung Objektname|
|InstanceName|PerfHourly|Windows Performance Zähler Instanznamen|
|CounteName|PerfHourly|Name der Zähler für Windows Leistungswerte|
|ObjectDisplayName|PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty|Anzeigename des Objekts Ziel von einer Leistung Websitesammlung Regel in Operations Manager oder der des Objekts, das durch Betrieb Einblicken oder für den die Benachrichtigung generiert wurde erkannt|
|RootObjectName|PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty|Anzeigename des übergeordneten Elements des übergeordneten (double Hostinganbieter Beziehung aufweisen: h. SqlDatabase von SqlInstance von Windows-Computer gehostet gehostet) des Objekts Ziel von einer Leistung Websitesammlung Regel in Operations Manager oder der des Objekts, das durch Betrieb Einblicken oder für den die Benachrichtigung generiert wurde erkannt|
|Computer|Die meisten Typen|Computernamen, die die Daten gehört|
|DeviceName|ProtectionStatus|Computernamen Daten gehört zum (identisch mit "Computer")|
|DetectionId|ProtectionStatus| |
|ThreatStatusRank|ProtectionStatus|Status Bedrohungsrang ist eine numerische Darstellung des Status Bedrohung und ähnliche HTTP-Antwortcodes, wir verlassen haben Lücken zwischen den Zahlen (also warum keine Risiken ist 150 und nicht 100 oder 0), damit wir etwas Platz zum Hinzufügen von neuer Staaten haben. In diesem Fall einen Rollup für Bedrohung Status- und Schutz wir den schlechtesten Status anzeigen möchten, den der Computer in den ausgewählten Zeitraum wurde. Wir verwenden die Zahlen, um die unterschiedlichen Zustände Rangfolge, damit wir für den Datensatz mit der höchsten Nummer suchen können.|
|ThreatStatus|ProtectionStatus|Beschreibung der ThreatStatus, ordnet die 1:1 mit ThreatStatusRank|
|TypeofProtection|ProtectionStatus|Anti-Malware-Produkt, das auf dem Computer erkannt wird: keine, Microsoft Schadsoftware Tools zum Entfernen, Forefront, usw.|
|ScanDate|ProtectionStatus| |
|SourceHealthServiceId|ProtectionStatus, RequiredUpdate|HealthService-ID für Agent des Computers|
|HealthServiceId|Die meisten Typen|HealthService-ID für Agent des Computers|
|"Verwaltungsgruppenname"|Die meisten Typen|Management Group unter Name für Agents Operations Manager angefügt; Andernfalls wird es Null/leer sein.|
|Objekttyp|ConfigurationObject|Typ (wie in Operations Manager Management-Sprachpaket 'Typ' / Klasse) für dieses Objekt von Log Analytics Konfiguration Bewertung erkannt|
|UpdateTitle|RequiredUpdate|Name des Updates, die gefunden wurde nicht installiert|
|PublishDate|RequiredUpdate|Wenn die Aktualisierung wurde veröffentlicht auf Microsoft Update?|
|Server|RequiredUpdate|Computernamen Daten gehört zum (identisch mit "Computer")|
|Produkt|RequiredUpdate|Produkt, das das Update gilt für|
|UpdateClassification|RequiredUpdate|Art des Updates (Updaterollup, Servicepacks usw.)|
|KBID|RequiredUpdate|KB-Artikel-ID, die diese bewährte Methode oder das Update beschrieben|
|WorkflowName|ConfigurationAlert|Name der Regel oder Monitor, die die Benachrichtigung erzeugt|
|Schwere|ConfigurationAlert|Schwere der Warnung|
|Priorität|ConfigurationAlert|Priorität der Warnung|
|IsMonitorAlert|ConfigurationAlert|Werden diese Warnung wird durch einen Monitor (WAHR) oder eine Regel (falsch) generiert?|
|AlertParameters|ConfigurationAlert|Mit den Parametern der Warnung Log Analytics XML|
|Kontextmenü|ConfigurationAlert|XML-Daten mit den Kontext der Warnung Log Analytics|
|Arbeitsbelastung|ConfigurationAlert|Technologie oder 'Arbeitsbelastung', die auf die Benachrichtigung verweist|
|AdvisorWorkload|Empfehlungen|Technologie oder 'Arbeitsbelastung', die auf die Empfehlungen verweist|
|Beschreibung|ConfigurationAlert|Beschreibung der Warnung (kurz)|
|DaysSinceLastUpdate|UpdateAgent|Wie viele Tage (relativ zum 'TimeGenerated' für diesen Datensatz) vor werden dieser Agent alle Aktualisieren von WSUS/Microsoft Update installiert?|
|DaysSinceLastUpdateBucket|UpdateAgent|Basierend auf DaysSinceLastUpdate, eine Kategorisierung in 'Perioden' von ein Computer vor wie langer Zeit wurde zuletzt alle Update installiert WSUS/Microsoft Update|
|AutomaticUpdateEnabled|UpdateAgent|Automatische Aktualisierung aktivieren aktiviert oder deaktiviert wird bei diesem Agent?
|AutomaticUpdateValue|UpdateAgent|Ist automatische Aktualisierung aktivieren festlegen automatisch herunterladen und installieren, nur herunterladen oder nur überprüfen?|
|WindowsUpdateAgentVersion|UpdateAgent|Die Versionsnummer des Microsoft Update-Agents|
|WSUSServer|UpdateAgent|Welche der zentrale ist dieses Update-Agent abgesehen?|
|OSVersion|UpdateAgent|Version des Betriebssystems wird dieses Update-Agent ausgeführt|
|Namen|Wird empfohlen, ConfigurationObjectProperty|/ Titel der Empfehlungen, oder der Name der Eigenschaft aus Log Analytics Konfiguration Bewertung|
|Wert|ConfigurationObjectProperty|Wert einer Eigenschaft aus Log Analytics Konfiguration Bewertung|
|KBLink|Empfehlungen|URL der KB-Artikel, der diese bewährte Methode oder das Update beschrieben|
|RecommendationStatus|Empfehlungen|Empfehlungen gehören die einige Arten, deren Datensätze 'aktualisiert abrufen' nicht nur an den Suchindex hinzugefügt, aus. Dieser Status ändert sich, ob es empfohlen aktiv/öffnen wird oder wenn Log Analytics erkennt, dass es behoben wurde.|
|RenderedDescription|Ereignis|Gerenderten Beschreibung (eingetragenen Parameter erneut verwendeten Text) ein Windows-Ereignis|
|ParameterXml|Ereignis|XML-Daten mit den Parametern im Abschnitt 'Daten' eines Ereignisses Windows (wie in der Ereignisanzeige dargestellt)|
|EventData|Ereignis|XML-Daten mit dem Abschnitt gesamte 'Daten' eines Ereignisses Windows (wie in der Ereignisanzeige dargestellt)|
|Datenquelle|Ereignis|Ereignisprotokoll-Quelle, die das Ereignis generiert.|
|EventCategory|Ereignis|Kategorie des Ereignisses, direkt aus dem Windows-Ereignisprotokoll|
|Benutzername|Ereignis|Benutzername des Windows-Ereignisses (in der Regel NT-AUTORITÄT\LocalSystem)|
|SampleValue|PerfHourly|Mittelwert der mittlere Wert für die Aggregation stündlich ein Performance-Zähler|
|Min|PerfHourly|Minimalwert im stündlich Intervall eines Leistung Zähler stündlich Aggregats|
|Max|PerfHourly|Maximalwert im stündlich Intervall eines Leistung Zähler stündlich Aggregats|
|Percentile95|PerfHourly|Der 95. Prozentwert für das Intervall stündlich eines Leistung Zähler stündlich Aggregats|
|SampleCount|PerfHourly|Wie viele 'unformatierten' Leistungsindikatorstichproben verwendet wurden, um diesen stündlich aggregieren Eintrag zu erstellen.|
|Bedrohung|ProtectionStatus|Name des Schadsoftware gefunden|
|StorageAccount|W3CIISLog|Azure-Speicher-Konto das Protokoll wurde aus lesen.|
|AzureDeploymentID|W3CIISLog|Azure-Bereitstellung ID des Cloud-Dienst das Protokoll gehört|
|Rolle|W3CIISLog|Rolle der Azure-Cloud-Dienst das Protokoll gehört|
|RoleInstance|W3CIISLog|RoleInstance der Azure-Rolle, die das Protokoll gehört|
|sSiteName|W3CIISLog|IIS-Website, die das Protokoll (Metabasis Notation); gehört das Feld s-Sitename in der ursprünglichen log|
|ComputernameIhresPartners|W3CIISLog|Das Feld s-Computername in der ursprünglichen log|
|sIP|W3CIISLog|Server IP-Adresse der HTTP-Anforderung wurde zu berücksichtigt. Das s-IP-Feld in der ursprünglichen log|
|csMethod|W3CIISLog|HTTP-Methode (GET/POST/usw.) der Client bei der HTTP-Anforderung. Die Protokollmethode in der ursprünglichen log|
|Überweisung|W3CIISLog|Client IP-Adresse der HTTP-Anforderung stammt. Das Feld Ip-c in der ursprünglichen log|
|csUserAgent|W3CIISLog|HTTP-Benutzer-Agents deklariert vom Client (Browser oder auf andere Weise). Cs-Benutzer-Agents in der ursprünglichen log|
|scStatus|W3CIISLog|HTTP-Status Code (200/403/500/usw.) vom Server an den Client zurückgegeben. Die Cs-Status in der ursprünglichen log|
|TimeTaken|W3CIISLog|Wie lange (in Millisekunden), die zum Abschließen benötigt die Anforderung wurde. Das Feld "Timetaken" in der ursprünglichen log|
|csUriStem|W3CIISLog|Relativer Uri (ohne Adresse, d. h. hosten ' / Suchen '), angefordert wurde. Das Feld "Cs-Uristem" in der ursprünglichen log|
|csUriQuery|W3CIISLog|URI-Abfrage. URI-Abfragen sind nur für dynamische Seiten, wie z. B. ASP-Seiten erforderlich, damit dieses Feld in der Regel einen Bindestrich für statische Seiten enthält.|
|Sportart|W3CIISLog|Serverport, die an die HTTP-Anforderung gesendet (und IIS überwacht, da es von ausgewählten)|
|csUserName|W3CIISLog|Benutzername authentifiziert, ist die Anforderung authentifizierte und nicht anonyme|
|csVersion|W3CIISLog|In der Anforderung verwendete HTTP-Protokoll-Version (d. h. ' HTTP / 1.1')|
|csCookie|W3CIISLog|Cookie-Informationen|
|csReferer|W3CIISLog|Website, die Besucher. Diese Website wurde ein Link für die aktuelle Website bereitgestellt.|
|csHost|W3CIISLog|Host Kopfzeile (d. h. ' www.mysite.com') angefordert wurde|
|scSubStatus|W3CIISLog|Untergeordneter Fehlercode:|
|scWin32Status|W3CIISLog|Windows-Status code|
|csBytes|W3CIISLog|In der Anforderung vom Client an den Server gesendeten Bytes|
|scBytes|W3CIISLog|Bytes wieder in die Antwort vom Server an den Client zurückgegeben|
|ConfigChangeType|ConfigurationChange|Art der Änderung (WindowsServices / Software / usw.)|
|ChangeCategory|ConfigurationChange|Kategorie der Änderung (geändert / hinzugefügt / entfernt)|
|SoftwareType|ConfigurationChange|Art von Software (Aktualisieren der Anwendung /)|
|SoftwareName|ConfigurationChange|Name der Software (gilt nur für Software Änderungen)|
|Publisher|ConfigurationChange|Lieferant, der die Software (gilt nur für Software Änderungen) veröffentlicht.|
|SvcChangeType|ConfigurationChange|Typ von Änderung, die auf einem Windows-Dienst angewendet wurde (Bundesstaat / StartupType / Pfad / Dienstkonto) – gilt nur für Windows-Dienst Änderungen|
|SvcDisplayName|ConfigurationChange|Anzeigename des Diensts, der geändert wurde|
|SvcName|ConfigurationChange|Name des Diensts, der geändert wurde|
|SvcState|ConfigurationChange|Neue (aktuelle) Status des Diensts|
|SvcPreviousState|ConfigurationChange|Vorherige bekannte Dienststatus (nur verfügbar, wenn Dienststatus geändert)|
|SvcStartupType|ConfigurationChange|Starttyp|
|SvcPreviousStartupType|ConfigurationChange|Vorherigen Starttyp (nur verfügbar, wenn Starttyp geändert)|
|SvcAccount|ConfigurationChange|Dienstkonto|
|SvcPreviousAccount|ConfigurationChange|Vorherigen Dienstkontos (nur verfügbar, wenn Dienstkonto geändert)|
|SvcPath|ConfigurationChange|Pfad für die ausführbare Datei des Windows-Diensts|
|SvcPreviousPath|ConfigurationChange|Vorherige Pfad des Programms für den Windows-Dienst (nur verfügbar, wenn es geändert)|
|SvcDescription|ConfigurationChange|Beschreibung des Diensts|
|Vorherigen|ConfigurationChange|Vorherigen Zustand dieser Software (installierten / nicht installierten / vorherigen Version)|
|Aktuelle|ConfigurationChange|Aktuellen Status dieser Software (installierten / nicht installierten / aktuelle Version)|

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Log Suchbegriffe:

- Kennenlernen Sie [Log Suchbegriffe](log-analytics-log-searches.md) , detaillierte von Lösungen gesammelten Informationen anzuzeigen.
- Verwenden Sie [benutzerdefinierte Felder in Log Analytics](log-analytics-custom-fields.md) um Log Suchbegriffe zu erweitern.
