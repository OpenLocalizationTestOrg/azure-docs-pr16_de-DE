<properties 
    pageTitle="SQL-Syntax und SQL-Abfrage für DocumentDB | Microsoft Azure" 
    description="SQL-Syntax, Datenbankkonzepte und SQL-Abfragen für DocumentDB, einer Datenbank NoSQL erfahren. SQL kann als eine JSON-Abfragesprache in DocumentDB verwendet." 
    keywords="SQL-Syntax, Sql-Abfrage, Sql-Abfragen, Json-Abfragesprache, Datenbankkonzepte und Sql-Aggregatfunktionen Abfragen"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-Abfrage und SQL-Syntax in DocumentDB
Microsoft Azure DocumentDB unterstützt Abfragen mithilfe von SQL (Structured Query Language) als eine JSON-Abfragesprache Dokumente. DocumentDB ist wirklich Schema-frei. Aufgrund sein Engagement in das JSON-Datenmodell direkt in die Datenbank-Engine bietet es automatische indizieren Dokumenten JSON ohne explizite Schema oder Erstellen von sekundären Indizes. 

Beim Entwerfen der Abfragesprache für DocumentDB hatten wir zwei Ziele vor Augen:

-   Statt gleichzeitig eine neue JSON-Abfragesprache, wollten SQL unterstützen. SQL ist eine der am häufigsten vertraut und beliebte Abfragesprachen. DocumentDB SQL stellt eine formale Programmierung für Rich-Abfragen über JSON-Dokumente.
-   Als JSON Dokument Datenbank JavaScript direkt in der Datenbank-Engine ausgeführt werden kann möchte JavaScript Programmierung Modell als Grundlage für unsere Abfragesprache verwenden. SQL-DocumentDB Ausgangspunkt in JavaScript Typsystem, Auswertung von Ausdrücken und Funktion aufrufen. Diese aktivieren bietet ein natürlich programming Modell für relationale Projektionen, hierarchische Navigation, über JSON-Dokumente, Self Verknüpfungen, raumbezogener Abfragen sowie das Aufrufen von benutzerdefinierten Funktionen (Functions, UDFs) vollständig in JavaScript, zwischen andere Features geschrieben. 

Wir glauben, dass diese Funktionen-Taste, um die Reibung zwischen der Anwendung und die Datenbank verringern und sind entscheidend für Entwicklerproduktivität.

Es empfiehlt sich, erste Schritte, durch das folgende Video an, wo Aravind Ramachandran DocumentDBs Abfragen Funktionen zeigt, beobachten und besuchen Sie unsere [Abfrage-Umgebung](http://www.documentdb.com/sql/demo), wo Sie DocumentDB testen und unser Dataset SQL-Abfragen ausführen können.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Klicken Sie dann zurück zum in diesem Artikel, in dem wir eine SQL-Abfrage-Lernprogramm beginnen mit, die Sie durch einige einfache JSON-Dokumente und SQL-Befehle geführt.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Erste Schritte mit SQL-Befehle in DocumentDB
Anzeigen von DocumentDB SQL bei arbeiten, lassen Sie uns beginnen mit wenigen einfachen JSON-Dokumente und einige einfachen Suchabfragen durchgehen. Betrachten Sie diese zwei JSON-Dokumente über zwei Familien aus. Beachten Sie, dass mit DocumentDB, wir nicht benötigte alle Schemas oder sekundäre Indizes explizit zu erstellen. Einfach müssen wir die JSON-Dokumente auf eine Auflistung DocumentDB einfügen und anschließend Abfragen. Hier haben wir eine einfache JSON Dokument für die Familie Andersen, die übergeordnete, untergeordnete (oder deren Tiere), Adresse und Registrierungsinformationen. Das Dokument enthält, Zeichenfolgen, Zahlen, boolesche Werte, Arrays und geschachtelte Eigenschaften. 

**Dokument**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Hier ist ein zweites Dokument mit ein feiner Unterschied – `givenName` und `familyName` anstelle von `firstName` und `lastName`.

**Dokument**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Nun versuchen wir einige Abfragen für diese Daten zu verstehen, einige der wichtigsten Aspekte des DocumentDB SQL. Beispielsweise die folgende Abfrage zurückgegeben werden kann die Dokumente, bei denen das Feld Id übereinstimmt `AndersenFamily`. Da es ist eine `SELECT *`, die Ausgabe der Abfrage wird das vollständige JSON-Dokument:

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Betrachten Sie nun den Fall, in dem Beginn wir die JSON-Ausgabe in eine andere Form formatieren müssen, aus. Diese Abfrage Projekte ein neues JSON-Objekt mit zwei markierten Felder Name und Ort, wenn die Adresse Ort denselben Namen als Status enthält. In diesem Fall entspricht "NY, NY" ein.

**Abfrage**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Ergebnisse**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Die nächste Abfrage gibt die angegebenen Namen der untergeordneten Elemente in der Familie, deren Id entspricht `WakefieldFamily` nach den Ort des Wohnorts sortiert.

**Abfrage**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Ergebnisse**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Wir möchten auf ein paar beachtenswert Aspekte der DocumentDB Abfragesprache durch die Beispiele aufmerksam, die wir bisher gesehen haben:  
 
-   Da DocumentDB SQL auf JSON-Werten zusammenarbeitet, befasst sich diese mit Strukturansicht Legende geformt Einheiten anstelle von Zeilen und Spalten aus. Daher können Sie auf Knoten der Struktur auf eine beliebige Tiefe, verweisen wie die Sprache mithilfe `Node1.Node2.Node3…..Nodem`, ähnlich wie relationale SQL verweisen auf den zweiteiligen Bezug der `<table>.<column>`.   
-   Die strukturierte Abfragesprache funktioniert mit Schema zugängliche Daten. Daher muss das Typsystem dynamisch gebunden werden soll. Der gleiche Ausdruck kann verschiedene Typen von anderen Dokumenten ergeben. Das Ergebnis einer Abfrage einen gültigen JSON-Wert ist, aber nicht unbedingt eine feste Schema aufweisen.  
-   DocumentDB unterstützt nur strict JSON-Dokumenten. Dies bedeutet, dass das System ein und Ausdrücke eingeschränkten JSON-Typen nur behandelt werden. Lizenzinformationen finden Sie weitere Details der [JSON-Spezifikation](http://www.json.org/) .  
-   Eine Auflistung von DocumentDB ist ein Schema frei Container JSON-Dokumente. Die Beziehungen in Daten Personen innerhalb und über Dokumente in einer Websitesammlung werden implizit erfasst, durch Eingrenzung und nicht durch den Primärschlüssel und Fremdschlüssel Key Beziehungen. Dies ist ein wichtiger Aspekt hingewiesen aufgrund der innerhalb eines Dokuments Verknüpfungen, die später in diesem Artikel beschrieben werden.

## <a name="documentdb-indexing"></a>DocumentDB Indizierung

Bevor wir die DocumentDB SQL-Syntax zu verschaffen, lohnt untersuchen den Indizierung Entwurf im DocumentDB. 

Die Datenbankindizes dient zum Abfragen in den verschiedenen Formen und Formen mit minimalen Ressourcenverbrauch (wie etwa CPU- und e/a) dienen beim Bereitstellen von guten Durchsatz und niedrige Wartezeit. Häufig erfordert die Auswahl des richtigen Index für eine Datenbankabfrage viel Planung und experimentieren. Dieser Ansatz stellt eine Herausforderung für Schema zugängliche Datenbanken, wo die Daten einer strict Schema nicht entsprechen, und schnell weiterentwickelt. 

Wenn wir die Indizierung Subsystem DocumentDB entworfen, legen wir deshalb die folgenden Ziele:

-   Indizieren von Dokumenten ohne Schema: Teilsystems Indizierung keine erfordert alle Schemainformationen oder keine Rückschlüsse Schema der Dokumente. 

-   Unterstützung für effiziente, umfangreichen hierarchische und relationale Abfragen: Index unterstützt die DocumentDB-Abfragesprache effizient, einschließlich der Unterstützung für hierarchische und relationale Projektionen.

-   Unterstützung für konsistente Abfragen bei Auftreten einer längeren Lautstärke schreibt: für hohe schreiben Durchsatz Auslastung mit konsistent Abfragen, der Index wird aktualisiert inkrementell, effizient und online unter einer längeren Lautstärke schreibt. Die konsistente Index aktualisieren ist entscheidend, die Abfragen Ebene der Konsistenz dienen, in dem der Benutzer den Dokumentdienst konfiguriert.

-   Unterstützung für mehrere Mandanten: angegebenen Basis Reservierung Modell für Governance Ressource über Mandanten, budgetgerecht Systemressourcen (CPU, Arbeitsspeicher und Eingabe-/Ausgabe-Vorgänge pro Sekunde) pro Replikation reserviert Aktualisierung von Indizes vorgenommen werden. 

-   Effizientes speichern: für Kosten Effektivität, ist der Speicherplatz auf dem Datenträger Aufwand des Indexes begrenzte und vorhersehbar. Dies ist entscheidend, da DocumentDB werden den Entwickler Kosten Basis Kompromisse zwischen Index Aufwand in Bezug auf die abfrageleistung vornehmen kann.  

So konfigurieren Sie die Indizierung Richtlinie für eine Websitesammlung mit Beispielen finden Sie in den [DocumentDB Beispiele](https://github.com/Azure/azure-documentdb-net) auf MSDN. Jetzt erfahren Sie in die Details der DocumentDB SQL-Syntax.


## <a name="basics-of-a-documentdb-sql-query"></a>Grundlagen einer DocumentDB SQL-Abfrage
Jeder Abfrage besteht aus einer SELECT-Klausel und optional FROM und WHERE-Klausel pro ANSI SQL Standards. Bei jeder Abfrage werden in der Regel, die Quelle in der FROM-Klausel aufgelistet. Dann wird der Filter in der WHERE-Klausel auf die Datenquelle verfügen, um eine Teilmenge der JSON-Dokumente abrufen angewendet. Schließlich wird die SELECT-Klausel verwendet, um die angeforderten JSON-Werte in der Auswahlliste project.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM-Klausel
Die `FROM <from_specification>` -Klausel ist optional, es sei denn, die Quelle gefiltert oder später in der Abfrage Erwarteter ist. Diese Klausel dient zum Festlegen der Datenquelle, von der die Abfrage ausgeführt werden muss. Im Allgemeinen wird die gesamte Sammlung der Quelle, aber eine Teilmenge der Sammlung können stattdessen angeben. 

Eine Abfrage wie `SELECT * FROM Families` gibt an, dass die gesamte Sammlung von Familien die Quelle für die aufgelistet ist. Eine spezielle Kennung ROOT kann zum Darstellen der Auflistung anstelle von den Namen der Websitesammlung verwendet werden. Die folgende Liste enthält die Regeln, die pro Abfrage erzwungen werden:

- Die Sammlung werden als Alias, wie `SELECT f.id FROM Families AS f` oder einfach `SELECT f.id FROM Families f`. Hier `f` entspricht dem `Families`. `AS`ist ein optionales Schlüsselwort Alias den Bezeichner enthält.

-   Hinweis das einmal Alias, die ursprüngliche Quelle kann nicht gebunden werden. Beispielsweise `SELECT Families.id FROM Families f` Syntax ungültig ist, da der Bezeichner "Familien" nicht mehr aufgelöst werden kann.

-   Alle Eigenschaften verwiesen werden sollen, müssen vollqualifiziert sein. Strict Schema Einhaltung fehlen ist dies erzwungen zur Vermeidung von einem beliebigen mehrdeutigen Bindungen. Daher `SELECT id FROM Families f` Syntax ungültig ist, da die Eigenschaft `id` nicht gebunden ist.
    
### <a name="sub-documents"></a>Untergeordnete Dokumente
Die Quelle kann auch auf eine kleinere Teilmenge reduziert werden. Beispielsweise konnte zum Auflisten von nur einer untergeordneten Struktur in jedem Dokument, Teilphasen Stammverzeichnis danach die Quelle werden wie im folgenden Beispiel gezeigt.

**Abfrage**

    SELECT * 
    FROM Families.children

**Ergebnisse**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Während der obige Beispiel ein Array als Quelle verwendet, konnte ein Objekt auch als Quelle, also wie im folgenden Beispiel dargestellt wird verwendet. Einen beliebigen gültiger JSON Wert (nicht undefiniert), der in der Quelle gefunden werden kann, wird für die Einbeziehung in das Ergebnis der Abfrage berücksichtigt. Wenn einige Familien besitzen eine `address.state` Wert, werden sie in das Abfrageergebnis ausgeschlossen werden.

**Abfrage**

    SELECT * 
    FROM Families.address.state

**Ergebnisse**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE-Klausel
Der WHERE-Klausel (**`WHERE <filter_condition>`**) ist optional. Es gibt an, dass die Filterausdrücke, die die Quelle der JSON-Dokumente genannt erfüllen muss, damit das Ergebnis enthalten sein. Die angegebenen Bedingungen auf "true" für das Ergebnis berücksichtigt werden muss jedes Dokument JSON ausgewertet werden. Die WHERE-Klausel wird durch den Index Layer verwendet, bei der Feststellung die absolute kleinste Teilmenge der von Quelldokumenten, die Teil des Ergebnisses werden können. 

Die folgende Abfrage anfordert, Dokumente, die Name-Eigenschaft enthalten, dessen Wert ist `AndersenFamily`. Alle anderen Dokumente, die nicht über eine Name-Eigenschaft verfügt oder, in denen der Wert nicht übereinstimmen `AndersenFamily` ausgeschlossen ist. 

**Abfrage**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Im vorherige Beispiel hat eine einfache Übereinstimmungsvergleiche Abfrage gezeigt. DocumentDB SQL unterstützt auch eine Vielzahl von skalaren Ausdrücken. Die am häufigsten verwendeten sind Binärzahl und unärer Ausdrücke. Verweise auf Eigenschaften aus den JSON-Quellobjekt sind auch gültige Ausdrücke. 

Die folgenden binären Operatoren werden aktuell unterstützt und können in Abfragen verwendet werden, wie in den folgenden Beispielen dargestellt:  
<table>
<tr>
<td>Arithmetische</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitweises</td>    
<td>|, &, ^, <<, >>, >>> (0 (null) – Füllung Verschiebung nach rechts) </td>
</tr>
<tr>
<td>Logisch</td>
<td>UND, ODER, NICHT</td>
</tr>
<tr>
<td>Vergleich</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Zeichenfolge</td> 
<td>|| (Verkettung)</td>
</tr>
</table>  

Werfen Sie einen Blick auf einige Abfragen mit binären Operatoren an.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Die Unäroperatoren +,-, ~ nicht werden ebenfalls unterstützt und können in Abfragen verwendet werden, wie im folgenden Beispiel gezeigt:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Zusätzlich zu Binärzahl und unärer Operatoren zulässig sind auch Verweise auf Eigenschaften. Beispielsweise `SELECT * FROM Families f WHERE f.isRegistered` gibt das JSON-Dokument mit der Eigenschaft `isRegistered` , in dem der Wert der Eigenschaft ist gleich der JSON `true` Wert. Alle anderen Werte (false, null, nicht definierten, `<number>`, `<string>`, `<object>`, `<array>`usw.) mit dem Quelldokument aus dem Ergebnis ausgeschlossen werden leads. 

### <a name="equality-and-comparison-operators"></a>Gleichheits-und Vergleichsoperator
Die folgende Tabelle zeigt das Ergebnis Gleichheitsvergleiche in SQL DocumentDB, zwischen zwei Typen JSON.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Aktion ausgeführt</strong>
         </td>
         <td valign="top">
            <strong>Nicht definiert</strong>
         </td>
         <td valign="top">
            <strong>NULL-Werten</strong>
         </td>
         <td valign="top">
            <strong>Boolesch</strong>
         </td>
         <td valign="top">
            <strong>Zahl</strong>
         </td>
         <td valign="top">
            <strong>Zeichenfolge</strong>
         </td>
         <td valign="top">
            <strong>Objekt</strong>
         </td>
         <td valign="top">
            <strong>Matrix</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nicht definiert<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>NULL-Werten<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Boolesch<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Zahl<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Zeichenfolge<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objekt<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Matrix<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
      </tr>
   </tbody>
</table>

Für andere Vergleichsoperatoren wie >, > =,! =, < und < =, die folgenden Regeln anwenden:   

-   Vergleich verschiedene Typen führt undefiniert.
-   Vergleich zwischen zwei Objekte oder zwei Matrizen undefiniert ergibt.   

Ist das Ergebnis des Ausdrucks skalaren im Filter nicht definiert ist, das entsprechende Dokument würde nicht enthalten sein im Ergebnis, da undefiniert logisch auf "True" entsprechen nicht.

### <a name="between-keyword"></a>ZWISCHEN Schlüsselwort
Das BETWEEN-Schlüsselwort können Sie auch Abfragen für Wertebereiche wie in ANSI SQL express. ZWISCHEN kann anhand von Zeichenfolgen oder Zahlen verwendet werden.

Beispielsweise gibt diese Abfrage alle familiäre Dokumente, bei denen das erste untergeordnete Noten zwischen 1 und 5 (beide einschließlich) ist. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Im Gegensatz zu in ANSI-SQL können auch die BETWEEN-Klausel in der FROM-Klausel wie im folgenden Beispiel Sie.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Für die Abfrage Ausführung schnelleres Denken Sie daran, eine Indizierung Richtlinie zu erstellen, die einen Bereich Index für numerische Eigenschaften/Pfade verwendet, die in die BETWEEN-Klausel gefiltert werden. 

Der wichtigste Unterschied zwischen der Verwendung von BETWEEN in DocumentDB und ANSI SQL ist, Sie Bereichsabfragen für Eigenschaften gemischte Datentypen express können – beispielsweise, möglicherweise Sie "Noten" eine Zahl (5) werden müssen in einigen Dokumente und Zeichenfolgen in andere ("grade4"). In diesen Fällen wird wie in JavaScript, ein Vergleich zwischen zwei verschiedenen Arten Ergebnisse in "Undefiniert", und das Dokument übersprungen.

### <a name="logical-and-or-and-not-operators"></a>Wahrheitswert (AND, OR und NOT) Operatoren
Logische Operatoren verwenden für boolesche Werte. In den folgenden Tabellen sind die logischen Wahrheitswert Tabellen für diese Operatoren aufgeführt.

ODER|WAHR|Falsch|Nicht definiert
---|---|---|---
WAHR|WAHR|WAHR|WAHR
Falsch|WAHR|Falsch|Nicht definiert
Nicht definiert|WAHR|Nicht definiert|Nicht definiert

UND|WAHR|Falsch|Nicht definiert
---|---|---|---
WAHR|WAHR|Falsch|Nicht definiert
Falsch|Falsch|Falsch|Falsch
Nicht definiert|Nicht definiert|Falsch|Nicht definiert

NICHT|  |
---|---
WAHR|Falsch
Falsch|WAHR
Nicht definiert|Nicht definiert

### <a name="in-keyword"></a>IN-Schlüsselwort
Prüfen, ob ein angegebener Wert einen beliebigen Wert in einer Liste entspricht, kann das Schlüsselwort IN verwendet werden. Beispielsweise gibt diese Abfrage alle familiäre Dokumente, wobei die Id, die eine "WakefieldFamily" oder "AndersenFamily" ist. 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

In diesem Beispiel gibt alle Dokumente Wo finde ich der Status eines der angegebenen Werte.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternär (?) und Operatoren zusammengesetzten (?)
Die Ternär und zusammengesetzten Operatoren können zum Erstellen von bedingten Ausdrücken, ähnlich wie beliebte programming Sprachen wie c# und JavaScript verwendet werden. 

Der Operator Ternär (?) kann sehr nützlich sein, beim Erstellen von neuer JSON-Eigenschaften im laufenden Betrieb. Jetzt beispielsweise können Sie Abfragen, um Class Ebenen in einem Menschen lesbare Form wie Anfänger/fortgeschrittene/erweiterte wie unten dargestellt zu klassifizieren schreiben.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Sie können auch die Anrufe an den Operator wie in der folgenden Abfrage schachteln.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Als werden mit anderen Abfrageoperatoren, wenn die Eigenschaften verwiesen wird im bedingten Ausdruck in jedem Dokument nicht vorhanden sind oder die zu vergleichenden Typen unterscheiden, werden diese Dokumente in den Abfrageergebnissen ausgeschlossen.

Der Operator zusammengesetzten (?) kann verwendet werden, effizient überprüfen auf das Vorhandensein einer Eigenschaft (auch bekannt als definiert ist) in einem Dokument. Dies ist nützlich, wenn gegen teilweise strukturierten Abfragen oder gemischte Datentypen. Diese Abfrage gibt beispielsweise die "Nachname", sofern vorhanden, oder der "Nachname" ist nicht vorhanden ist.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>In Anführungszeichen Eigenschaftenaccessor
Sie können auch auf Eigenschaften, die mit dem Eigenschaftenoperator in Anführungszeichen zugreifen `[]`. Beispielsweise `SELECT c.grade` und `SELECT c["grade"]` entsprechen. Diese Syntax ist nützlich, wenn Sie eine Eigenschaft escape, die Leerzeichen, Sonderzeichen enthält oder geschieht mit demselben Namen wie ein SQL-Schlüsselwort oder einen reserviertes Wort freigeben müssen.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT-Klausel
Die SELECT-Klausel (**`SELECT <select_list>`**) ist obligatorisch und gibt an, welche Werte werden aus der Abfrage abgerufen werden, wie in ANSI-SQL. Die Teilmenge, die auf die Quelldokumente gefiltert werden auf der Projektionsphase, wo die angegebene JSON-Werte werden abgerufen und ein neues JSON-Objekt erstellt wird, für jede Eingabe auf diesem übergeben übergeben. 

Im folgenden Beispiel wird eine typische Auswahlabfrage. 

**Abfrage**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Geschachtelte Eigenschaften
Im folgenden Beispiel werden wir Prognostizieren von zwei geschachtelte Eigenschaften `f.address.state` und `f.address.city`.

**Abfrage**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projektion unterstützt auch JSON-Ausdrücke, wie im folgenden Beispiel dargestellt.

**Abfrage**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Wir wollen nun die Rolle des `$1` hier. Die `SELECT` -Klausel muss ein JSON-Objekt zu erstellen und da kein Schlüssel bereitgestellt wird, verwenden wir implizite Argument Variablennamen angefangen `$1`. Beispielsweise gibt diese Abfrage zwei implizite Argument Variablen, mit der Bezeichnung `$1` und `$2`.

**Abfrage**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Alias
Jetzt wir Erweitern von Werten im Beispiel oben mit expliziter Alias. Ist das Schlüsselwort für Alias verwendet. Beachten Sie, dass es optional siehe während ein Projektor angeschlossen ist als den zweiten Wert ist `NameInfo`. 

Für den Fall, dass eine Abfrage zwei Eigenschaften mit demselben Namen enthält, muss Alias verwendet werden, um eine oder beide der Eigenschaften umbenennen, damit sie in der geplanten Ergebnis auseinander gehalten werden.

**Abfrage**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalaren Ausdrücken
Neben Verweise auf Eigenschaften unterstützt die SELECT-Klausel auch skalare Ausdrücken wie Konstanten, arithmetische Ausdrücke, logischen Ausdrücke aus. So sieht beispielsweise eine einfache "Hallo Welt" Abfrage aus.

**Abfrage**

    SELECT "Hello World"

**Ergebnisse**

    [{
      "$1": "Hello World"
    }]


Hier ist ein komplexeres Beispiel, bei das einen Skalarausdruck verwendet.

**Abfrage**

    SELECT ((2 + 11 % 7)-2)/3   

**Ergebnisse**

    [{
      "$1": 1.33333
    }]


Im folgenden Beispiel ist das Ergebnis des Ausdrucks skalaren ein boolescher.

**Abfrage**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Ergebnisse**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Erstellen von Objekt und Matrix
Eine weitere wichtige Funktion von DocumentDB SQL ist Array-Objekt erstellen. Beachten Sie im vorherigen Beispiel erstellt ein neues JSON-Objekt aus. Eine kann auf ähnliche Weise auch Arrays erstellt, wie in den folgenden Beispielen dargestellt.

**Abfrage**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Ergebnisse**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>Wert Schlüsselwort
Das Schlüsselwort **Wert** bietet eine Möglichkeit zum JSON-Wert zurück. Beispielsweise gibt die Abfrage abgebildet der Skalarwert `"Hello World"` anstelle von `{$1: "Hello World"}`.

**Abfrage**

    SELECT VALUE "Hello World"

**Ergebnisse**

    [
      "Hello World"
    ]


Die folgende Abfrage gibt den JSON-Wert, ohne die `"address"` Bezeichnung in den Ergebnissen.

**Abfrage**

    SELECT VALUE f.address
    FROM Families f 

**Ergebnisse**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Das folgende Beispiel erweitert dies zum Veranschaulichen der zurückzugebenden primitive JSON-Werte (der untergeordneten Ebene der JSON-Struktur). 

**Abfrage**

    SELECT VALUE f.address.state
    FROM Families f 

**Ergebnisse**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operator
Der spezielle Operator (*) wird unterstützt, um das Dokument als project-ist. Wenn verwendet wird, muss es der einzige geplanten Feld sein. Während eine Abfrage wie `SELECT * FROM Families f` ist gültig, `SELECT VALUE * FROM Families f ` und `SELECT *, f.id FROM Families f ` sind nicht zulässig.

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>TOP-Operator
Das TOP-Schlüsselwort kann verwendet werden, die Anzahl der Werte aus einer Abfrage einschränken. Wenn oben in Verbindung mit der ORDER BY-Klausel verwendet wird, ist das Resultset auf die erste N-Anzahl von Ziffern oder Buchstaben sortierte Werte beschränkt. Andernfalls wird die erste N-Anzahl der Ergebnisse in einer nicht definierten Reihenfolge zurückgegeben. Als bewährte Methode in einer SELECT-Anweisung immer verwenden Sie eine ORDER BY-Klausel mit der TOP-Klausel. Dies ist die einzige Möglichkeit zum vorhersehbar anzugeben, welche Zeilen von oben betroffen sind. 


**Abfrage**

    SELECT TOP 1 * 
    FROM Families f 

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

TOP kann ein konstanter Wert (siehe oben) oder eine Variable Wert mit parametrisierte Abfragen verwendet werden. Weitere Informationen hierzu finden Sie unter folgenden parametrisierte Abfragen.

## <a name="order-by-clause"></a>ORDER BY-Klausel
Wie in ANSI-SQL, eine optionale Order By-Klausel beim Abfragen enthalten sein können. Die-Klausel kann ein optionales ASC/DESC-Argument, um die Reihenfolge festzulegen, in der die Ergebnisse abgerufen werden müssen, enthalten. Eine ausführlichere Betrachtung Order By finden Sie unter [DocumentDB Reihenfolge nach Exemplarische Vorgehensweise](documentdb-orderby.md).

Hier wird beispielsweise eine Abfrage, in der Reihenfolge der resident Ortsname Familien abruft.

**Abfrage**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Ergebnisse**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

Und hier eine Abfrage, die Familien in der Reihenfolge der Erstellungsdatum, die als eine Zahl, die Epoche darstellt gespeichert ist, ruft Anzeigedauer, wie z. B. verstrichene Zeit seit dem 1. Jan 1970 in Sekunden.

**Abfrage**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Ergebnisse**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Erweiterte Datenbankkonzepte und SQL-Abfragen
### <a name="iteration"></a>Iteration
Einen neuen erstellen wurde hinzugefügt, über das Schlüsselwort **IN** in SQL DocumentDB, um Unterstützung für Durchlaufen von JSON-Arrays zu bieten. Die Quelle von bietet Unterstützung für Iteration. Beginnen wir mit dem folgenden Beispiel:

**Abfrage**

    SELECT * 
    FROM Families.children

**Ergebnisse**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Jetzt wollen wir eine andere Abfrage, die Iteration für untergeordnete Elemente in der Websitesammlung ausführt. Beachten Sie den Unterschied in der Matrix. In diesem Beispiel wird geteilt `children` und die Ergebnisse in einem einzigen Array fasst.  

**Abfrage**

    SELECT * 
    FROM c IN Families.children

**Ergebnisse**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Dies kann weiter nach dem jeder einzelne Eintrag in der Matrix gefiltert werden soll, wie im folgenden Beispiel gezeigt verwendet werden.

**Abfrage**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Ergebnisse**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Verknüpfungen
In einer relationalen Datenbank unbedingt teilnehmen über Tabellen werden müssen. Es ist die logische Begleiterscheinung weiterentwickelt zu standardisierten Schemas zu entwerfen. Befasst sich im Gegensatz zu diesem DocumentDB mit dem Modell denormalisierte Daten von Dokumenten Schema frei. Dies ist die logische Entsprechung von a "Selbstjoins".

Die Syntax, die die Sprache unterstützt wird < from_source1 > Verknüpfung < from_source2 > Verknüpfung... Teilnehmen an < From_sourceN >. Insgesamt gibt dies eine Reihe von **N**- Tupel (Tupel mit **N** Werten). Jedes Tupels enthält Werte, die entsprechenden Datensätze durchlaufen alle Websitesammlung Aliase erzeugt. Kurzum, ist dies eine vollständige Kreuzprodukt der Teilnahme an der Verknüpfung Mengen.

In den folgenden Beispielen Funktionsweise der JOIN-Klausel. Im folgenden Beispiel ist das Ergebnis leer seit das Kreuzprodukt jedes Dokument von Quelle und ein leerer Satz leer ist.

**Abfrage**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Ergebnisse**  

    [{
    }]


Im folgenden Beispiel wird die Verknüpfung zwischen der Stammwebsitesammlung Dokument und die `children` untergeordnete aus. Es ist ein Kreuzprodukt zwischen zwei JSON-Objekte. Dass untergeordnete ist ein Array ist nicht effektiv in die Verknüpfung auf, da wir die Quadratwurzel einer einzelnen zuständig sind, die die untergeordneten Elementen Matrix ist. Daher enthält das Ergebnis nur zwei Ergebnisse, da das Kreuzprodukt jedes Dokument mit der Matrix genau nur ein Dokument ergibt.

**Abfrage**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Ergebnisse**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Im folgenden Beispiel wird eine weitere herkömmliche Verknüpfung:

**Abfrage**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Ergebnisse**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Der erste ist zu beachten, die die `from_source` von der **Teilnahme an** -Klausel ein Iterator ist. Ja, ist der Fluss in diesem Fall wie folgt:  

-   Erweitern Sie jede untergeordnetes Element **c** in der Matrix.
-   Anwenden einer Kreuzprodukt mit dem Stamm des Dokuments **d** mit jeder untergeordnetes Element **c** , die im ersten Schritt reduziert sind wurde.
-   Schließlich project die Quadratwurzel Objekt **f** Name-Eigenschaft alleine. 

Das erste Dokument (`AndersenFamily`) enthält nur ein untergeordnetes Element aus, damit Ergebnisgruppe nur ein einzelnes Objekt entspricht dieses Dokument enthält. Im zweiten Dokument (`WakefieldFamily`) enthält zwei untergeordneten Elementen. Ja, führt das Kreuzprodukt ein separates Objekt für jedes Kind, wodurch zwei Objekte, eine für jede untergeordnete, dieses Dokument entspricht. Beachten Sie, dass Root Felder in dieser beide Dokumente genauso wie Sie in ein Kreuzprodukt erwartet derselben.

Die real Programm die Verknüpfung besteht darin, Formular Tupel aus dem Kreuzprodukt in einer Form, die andernfalls schwer zu Project ist. Darüber hinaus wie wir im folgenden Beispiel sehen werden, können Sie auf die Kombination eines Tupels filtern, ermöglicht der Benutzer eine Bedingung erfüllt, die Tupel insgesamt ausgewählt haben.

**Abfrage**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Ergebnisse**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



In diesem Beispiel wird eine natürliche Erweiterung im obigen Beispiel und führt eine doppelte Verknüpfung. Ja, kann das Kreuzprodukt wie im folgenden Pseudocode angezeigt werden.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`hat ein Kind, ein Haustier hat. Ja, das Kreuzprodukt ergibt eine Zeile (1*1*1) aus dieser Familie. WakefieldFamily jedoch zwei untergeordnete Elemente hat, jedoch nur ein untergeordnetes Element "Jesse" Tiere. Jesse weist 2 Tiere durch. Daher ergibt das Kreuzprodukt 1*1*, 2 = 2 Zeilen aus dieser Familie.

Im nächsten Beispiel auf ein zusätzlicher Filter vorhanden ist `pet`. Dies schließt alle Tupel, denen nicht die Tiername "Schatten" ist. Beachten Sie, dass wir sind in der Lage, zum Erstellen von Arrays, filtern, klicken Sie auf eines der Elemente des Tupels, Tupel und project eine beliebige Kombination der Elemente. 

**Abfrage**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Ergebnisse**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript-integration
DocumentDB bietet ein programming Modell für die Ausführung von JavaScript-basierte Anwendungslogik direkt auf die Websitesammlungen in Bezug auf die gespeicherten Prozeduren und Triggern. Dadurch wird für beide:

-   Die Möglichkeit, Transaktionen Vorgänge hohe Leistung und Abfragen für Dokumente in einer Websitesammlung aufgrund der Tiefe Integration von JavaScript-Laufzeit direkt in die Datenbank-Engine vorzunehmen. 
-   Eine natürliche Modellierung von Fluss Steuerelements, Variable Bereichsdefinition, und Zuordnung und Integration von Ausnahme Behandlung anderer primitive mit Datenbanktransaktionen. Weitere Informationen über die DocumentDB-Unterstützung für JavaScript-Integration finden Sie in der Dokumentation JavaScript Server Seite Programmierbarkeit.

###<a name="user-defined-functions-udfs"></a>Benutzerdefinierte Funktionen (Functions, UDFs)
Mit den Typen bereits in diesem Artikel festgelegte bietet DocumentDB SQL Unterstützung für Benutzer benutzerdefinierte Funktionen (UDFs). Insbesondere werden skalaren UDFs unterstützt, wo die Entwickler in NULL oder viele Argumente übergeben und wieder ein einzelnes Argument Ergebnis zurückgeben können. Jedes dieser Argumente werden überprüft für rechtliche JSON-Werte werden.  

Die DocumentDB SQL-Syntax wird erweitert, um Unterstützung für benutzerdefinierte Anwendungslogik verwenden diese User Defined Functions. UDFs mit DocumentDB registriert werden können, und klicken Sie dann als Teil einer SQL-Abfrage verwiesen werden. Tatsächlich dienen die UDFs exquisitely von Abfragen geltend gemacht werden. Daneben für diese Auswahl haben UDFs keinen Zugriff auf das Kontextobjekt der den anderen JavaScript (gespeicherten Prozeduren und Trigger) aufweisen. Da Abfragen im schreibgeschützten Modus ausgeführt werden, können sie auf dem primären oder sekundären Replikate ausgeführt. Daher sollen UDFs auf sekundäre Replikate im Gegensatz zu anderen Typen von JavaScript ausgeführt werden.

Es folgt ein Beispiel für wie UDFs in der Datenbank DocumentDB speziell unter eine Auflistung Dokument registriert werden kann.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Im vorherige Beispiel erstellt, deren Name wird UDFs `REGEX_MATCH`. Er akzeptiert zwei JSON-Zeichenfolgenwerte `input` und `pattern` und überprüft, ob der ersten entspricht das Muster in der zweiten angegeben mithilfe JavaScripts-string.match()-Funktion.


Wir können jetzt diese UDFs in einer Abfrage in eine Projektion verwenden. UDFs müssen qualifiziert werden, mit der Groß-/Kleinschreibung beachtet Präfix "UDFs." Wenn Sie Abfragen aus aufgerufen. 

>[AZURE.NOTE] Vor 3/17/2015 unterstützt DocumentDB UDFs Anrufe ohne "UDFs." Präfix wie REGEX_MATCH() auswählen. Dieses einen Muster ist veraltet.  

**Abfrage**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Ergebnisse**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

UDFs kann auch innerhalb eines Filters verwendet werden, wie im folgenden Beispiel gezeigt qualifiziert auch mit "UDFs." Präfix:

**Abfrage**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Im Wesentlichen UDFs sind gültige skalaren Ausdrücken und sowohl Projektionen und Filter verwendet werden können. 

Um die Leistungsfähigkeit von UDFs zu erweitern, sehen wir uns ein weiteres Beispiel für bedingte Logik:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Im folgenden finden Sie ein Beispiel, die UDFs ausführt.

**Abfrage**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Ergebnisse**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Wie die vorherigen Beispielen verdeutlichen, integrieren UDFs die Leistungsfähigkeit von JavaScript-Sprache mit dem SQL-DocumentDB bietet eine programmierbare Rich-Oberfläche um komplexe zu Vorgehensweisen, bedingte Logik mit Hilfe der integrierten JavaScript-Laufzeitfunktionen auszuführen.

DocumentDB SQL stellt die Argumente in die UDFs für jedes Dokument in der Quelle auf der aktuellen Stufe der Verarbeitung UDFs (WHERE-Klausel oder SELECT-Klausel). Das Ergebnis wird in der gesamten Ausführung Verkaufspipeline nahtlos integriert. Wenn die Eigenschaften bezeichnet durch UDFs Parameter stehen nicht zur Verfügung, in den JSON-Wert, der Parameter gilt als nicht definiert und daher UDFs aufrufen vollständig übersprungen. Auf ähnliche Weise ist das Ergebnis des UDFs nicht definiert ist, nicht im Ergebnis enthalten. 

UDFs sind nützliche Tools komplexe Geschäftslogik als Teil der Abfrage ausführen.

### <a name="operator-evaluation"></a>Operator Auswertung
DocumentDB, zeichnet der Vorteil, dass es wird eine Datenbank JSON parallelen mit JavaScript-Operatoren und dessen Auswertung Semantik. Während der DocumentDB versucht, JavaScript im Hinblick auf JSON Support Semantik vorhanden sein, weicht die Bewertung der Vorgang in einigen Fällen ab.

In SQL DocumentDB sind im Gegensatz zu in herkömmlichen SQL, die Typen von Werten häufig nicht bekannt, bis die Werte aus einer Datenbank tatsächlich abgerufen werden. Um Abfragen effektiv ausführen zu können, müssen die meisten der Operatoren strict Typ Anforderungen an. 

DocumentDB SQL ausführen keine implizite Konvertierungen, im Gegensatz zu JavaScript. Eine Abfrage beispielsweise wie `SELECT * FROM Person p WHERE p.Age = 21` Dokumente, die eine Eigenschaft Alter enthalten, dessen Wert 21 ist, entspricht. Wie jedes andere Dokument, dessen Eigenschaft Alter Zeichenfolge "21" oder eine andere oftmals unbegrenzte Variationen entspricht, "021", "21.0", "0021", "00021", usw. nicht übereinstimmend steht. Dies ist im Gegensatz dazu an der Stelle, an der die Zeichenfolgenwerte implizit in Zahlen umgewandelt werden JavaScript (basierend auf Operator, ex: ==). Diese Auswahl ist entscheidend für den effizienten Index in SQL DocumentDB passenden. 

## <a name="parameterized-sql-queries"></a>Parametrisierte SQL-Abfragen
DocumentDB unterstützt Abfragen mit Parametern mit der gewohnten ausgedrückt @ Notation. Parametrisierte SQL bietet robuste Behandlung und Escapezeichen der Benutzereingabe verhindern unbeabsichtigtes Anzeigen von Daten über SQL einfügen. 

Sie können beispielsweise schreiben eine Abfrage, die als Parameter Nachname und Adresszustand annimmt, und führen Sie sie für die verschiedenen Werte Nachname und Adresszustand je nach Benutzereingabe.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Diese Anforderung kann dann an DocumentDB als eine Parameterabfrage JSON wie gesendet abgebildet.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Das Argument nach oben kann abgebildet festgelegt werden wie parametrisierte Abfragen verwenden.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwerte können gültigen JSON sein (Zeichenfolgen, Zahlen, boolesche Werte null, sogar Arrays oder verschachtelte JSON). Da DocumentDB Schema-kleiner ist, werden Parameter auch nicht anhand eines Typs überprüft.

##<a name="built-in-functions"></a>Integrierte Funktionen
DocumentDB unterstützt auch eine Reihe von integrierte Funktionen für grundlegende Vorgänge, die innerhalb der Abfragen, wie benutzerdefinierte Funktionen (Functions, UDFs) verwendet werden können.

<table>
<tr>
<td>Mathematische Funktionen</td> 
<td>ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, melden Sie sich, Wurzel, QUADRAT, kürzen, ARCCOS, ARCSIN, ARCTAN, ATN2, COS, COT, Grad, PI, Bogenmaß (Radiant), SIN und TAN</td>
</tr>
<tr>
<td>Geben Sie bei der Überprüfung von Funktionen</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED und IS_PRIMITIVE</td>
</tr>
<tr>
<td>Zeichenfolgenfunktionen</td>   
<td>VERKETTEN, enthält, ENDSWITH, INDEX_OF, links, Länge, unteren, LTrim-, ersetzen, REPLIZIERT, umgekehrte Richtung, rechts, RTrim-, STARTSWITH, TEILZEICHENFOLGE und oberen</td>
</tr>
<tr>
<td>Array-Funktionen</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH und ARRAY_SLICE</td>
</tr>
<tr>
<td>Räumliche Funktionen</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID und ST_ISVALIDDETAILED</td>
</tr>
</table>  

Wenn Sie aktuell eine benutzerdefinierte Funktion (UDFs) für die jetzt eine integrierte Funktion verfügbar ist verwenden, sollten Sie die entsprechende integrierte Funktion verwenden, wie es schneller geht ausgeführt werden und effizienter. 

### <a name="mathematical-functions"></a>Mathematische Funktionen
Mathematischen Funktionen führen Sie eine Berechnung, in der Regel basierend auf Eingabewerte, die als Argumente angegeben sind, und einen numerischen Wert zurückgeben. Die nachstehende Tabelle der unterstützten integrierten mathematischen Funktionen.

<table>
<tr>
<td><strong>Verwendung</strong></td>
<td><strong>Beschreibung</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (Num_expr)</a></td> 
<td>Gibt den absoluten (positiven) Wert des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">Obergrenze (Num_expr)</a></td> 
<td>Gibt den kleinsten Wert für die ganze Zahl, größer als oder gleich dem angegebenen numerischen Ausdruck.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">Untergrenze (Num_expr)</a></td> 
<td>Gibt der größten ganze Zahl kleiner oder gleich dem angegebenen numerischen Ausdruck.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (Num_expr)</a></td> 
<td>Gibt den angegebenen numerischen Ausdruck Exponent.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (Num_expr [, Basis])</a></td> 
<td>Gibt den natürlichen Logarithmus der angegebenen numerischen Ausdruck oder den Logarithmus die angegebene Basis verwenden</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (Num_expr)</a></td> 
<td>Gibt den Wert der Basis 10 logarithmischen des angegebenen numerischen Ausdrucks.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">RUNDEN (Num_expr)</a></td> 
<td>Gibt einen numerischen Wert, der auf die nächste ganze Zahl gerundet.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">Kürzen (Num_expr)</a></td> 
<td>Gibt einen numerischen Wert, der auf die nächste ganze Zahl gekürzt.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">Wurzel (Num_expr)</a></td>   
<td>Gibt die Wurzel aus der angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">Wurzel (Num_expr)</a></td>   
<td>Gibt das Quadrat des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (Num_expr, Num_expr)</a></td>   
<td>Gibt das angegebene Wert mit dem angegebenen numerischen Ausdruck zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">Melden Sie sich (Num_expr)</a></td>   
<td>Melden Sie sich gibt den Wert zurück (1, 0, 1) dem angegebenen numerischen Ausdruck.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ARCCOS (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, dessen Kosinus dem angegebenen numerischen Ausdruck ist; auch Arkuskosinus genannt.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ARCSIN (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, dessen Sinus dem angegebenen numerischen Ausdruck ist. Dies ist auch Arkussinus bezeichnet.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ARCTAN (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, dessen Tangens dem angegebenen numerischen Ausdruck ist. Dies ist auch Arkustangens bezeichnet.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, zwischen die positive x- und das mit und goldenen vom Ursprung zum Punkt (y, X), wobei x und y die Werte der beiden angegebenen Pufferzeiten Ausdrücke sind.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (Num_expr)</a></td> 
<td>Gibt den trigonometrischen Kosinus des angegebenen Winkels im Bogenmaß, in dem angegebenen Ausdruck.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (Num_expr)</a></td> 
<td>Gibt den trigonometrischen Kotangens eines angegebenen Winkels im Bogenmaß, in dem angegebenen numerischen Ausdruck.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">Grad (Num_expr)</a></td> 
<td>Gibt den entsprechenden Winkel in Grad für eines im Bogenmaß angegebenen Winkels zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Gibt den Wert von Pi zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">BOGENMASS (Num_expr)</a></td> 
<td>Gibt Bogenmaß (Radiant) zurück, wenn numerischer Ausdruck in Grad, eingegeben wird.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (Num_expr)</a></td> 
<td>Gibt den trigonometrischen Sinus eines angegebenen Winkels im Bogenmaß, in dem angegebenen Ausdruck.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (Num_expr)</a></td> 
<td>Gibt den Tangens der eingegebene Ausdruck, in dem angegebenen Ausdruck.</td>
</tr>

</table> 

Beispielsweise können Sie Abfragen, wie die folgende ausführen:

**Abfrage**

    SELECT VALUE ABS(-4)

**Ergebnisse**

    [4]

Der wichtigste Unterschied zwischen DocumentDBs Funktionen im Vergleich zu ANSI SQL ist, dass sie für die Arbeit mit Schemadaten zu öffnendes Schema und gemischten auch ausgelegt sind. Angenommen, wenn Sie ein Dokument haben, in dem die Eigenschaft Größe fehlt oder weist, einen nicht numerischen Wert wie "Unbekannt", und dann das Dokument wird übersprungen, statt einen Fehler zurückgeben.

### <a name="type-checking-functions"></a>Geben Sie bei der Überprüfung von Funktionen
Die Funktionen des Typs fehlerüberprüfung können Sie zum Überprüfen des Typs eines Ausdrucks in SQL-Abfragen. Bestimmen Sie den Typ der Eigenschaften innerhalb von Dokumenten im laufenden Betrieb Variables oder unbekannten angezeigt werden, können Funktionen bei der Überprüfung des Typs verwendet werden. Die nachstehende Tabelle unterstützten integrierten Typs Funktionen aktivieren.

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, ist der Typ des Werts einer Matrix zurück.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, ist der Typ des Werts ein boolescher Ausdruck.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, wenn der Typ des Werts null ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, wenn der Typ des Werts eine Zahl ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, wenn der Typ des Werts ein JSON-Objekt ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Typ des Werts einer Zeichenfolge ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, ob die Eigenschaft einen Wert zugewiesen wurde.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (Ausdruck)</a></td>
  <td>Gibt einen Boolean zurück, der angibt, wenn der Typ des Werts String, Zahl, Boolesch oder Null ist.</td>
</tr>

</table>

Mit den folgenden Funktionen, können Sie jetzt Abfragen wie folgt ausführen:

**Abfrage**

    SELECT VALUE IS_NUMBER(-4)

**Ergebnisse**

    [true]

### <a name="string-functions"></a>Zeichenfolgenfunktionen
Die folgenden Skalarfunktionen ein Vorgang für eine Zeichenfolge Eingabewerts und eine Zeichenfolge, die numerische oder boolesche Wert zurückgeben. Hier ist eine Tabelle mit integrierten Zeichenfolgenfunktionen aus:

Verwendung|Beschreibung
---|---
[Länge (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Gibt die Anzahl der Zeichen von der angegebenen Zeichenfolgenausdruck
[Verketten (Str_expr, Str_expr [, Str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Gibt eine Zeichenfolge, die das Ergebnis Verketten von zwei oder mehr Zeichenfolgenwerte ist.
[TEILZEICHENFOLGE (Str_expr, Num_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Gibt Teil Zeichenfolgenausdruck.
[STARTSWITH (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Gibt eine boolesche, die angibt, ob die erste Ausdruck Zeichenfolge endet mit der zweiten
[ENDSWITH (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Gibt eine boolesche, die angibt, ob die erste Ausdruck Zeichenfolge endet mit der zweiten
[ENTHÄLT (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Gibt eine boolesche, die angibt, ob die erste Ausdruck Zeichenfolge enthält die zweite.
[INDEX_OF (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Gibt die Position des ersten Auftretens des zweiten Zeichenfolgenausdruck innerhalb der ersten angegebenen Zeichenfolgenausdruck oder -1, wenn die Zeichenfolge nicht gefunden wird.
[LEFT (Str_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Gibt den linken Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen.
[RIGHT (Str_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Gibt den rechten Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen.
[LTRIM (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Gibt einen Zeichenfolgenausdruck ohne vorangestellte Leerzeichen zurück.
[RTRIM (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Gibt einen Zeichenfolgenausdruck, nachdem alle nachfolgenden Leerzeichen abgeschnitten.
[KLEIN (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Gibt einen Zeichenfolgenausdruck nach Zeichen von Großbuchstaben in Kleinbuchstaben um.
[OBERE (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Gibt einen Zeichenfolgenausdruck nach der Konvertierung Kleinbuchstaben in Großbuchstaben um.
[Ersetzen (Str_expr, Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Ersetzt alle Vorkommen einer angegebenen Zeichenfolgenwert durch einen anderen Zeichenfolgenwert an.
[Die Replikation (Str_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Wiederholt einem Zeichenfolgenwert eine angegebene Anzahl von Zeiten.
[Umgekehrte Richtung (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Gibt die umgekehrte Reihenfolge der ein Zeichenfolgenwert an.

Mit den folgenden Funktionen, können Sie jetzt wie folgt Abfragen ausführen. Beispielsweise können Sie den Namen der Familie in Großbuchstaben wie folgt zurückkehren:

**Abfrage**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Ergebnisse**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Oder Verketten von Zeichenfolgen wie in diesem Beispiel:

**Abfrage**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Zeichenfolgenfunktionen können auch in der WHERE-Klausel zum Filtern von Ergebnissen, wie im folgenden Beispiel verwendet werden:

**Abfrage**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Array-Funktionen
Die folgenden Skalarfunktionen ausführen ein Vorgangs auf eine Matrix Eingabewerts und numerische zurück, Boolesch oder einem Array Wert an. Hier ist eine Tabelle mit integrierten Matrixfunktionen aus:

Verwendung|Beschreibung
---|---
[ARRAY_LENGTH (Arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Gibt die Anzahl von Elementen des Ausdrucks angegebenen Arrays.
[ARRAY_CONCAT (Arr_expr, Arr_expr, Arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Gibt eine Matrix, die das Ergebnis zwei oder mehr Matrix Werte verkettet ist.
[ARRAY_CONTAINS (Arr_expr, Ausdruck)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Gibt einen Boolean zurück, der angibt, ob die Matrix den angegebenen Wert enthält.
[ARRAY_SLICE (Arr_expr, Num_expr, Num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Gibt Teil eines Ausdrucks Matrix zurück.

Matrixfunktionen können zur Bearbeitung von Arrays in JSON verwendet werden. So sieht beispielsweise eine Abfrage, die alle Dokumente zurückgegeben, ist eine der übergeordneten Elemente "Robin Wakefield" aus. 

**Abfrage**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Hier ist ein weiteres Beispiel, das ARRAY_LENGTH wird verwendet, um die Anzahl der untergeordneten Elemente pro Familie zu ermitteln.

**Abfrage**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Räumliche Funktionen

DocumentDB unterstützt die folgenden öffnen Geodaten Consortium (OGC) integrierte Funktionen für Geodaten Abfragen. Detaillierte Informationen zur Geodaten in DocumentDB unterstützen Sie, finden Sie unter [Arbeiten mit Geodaten in Azure DocumentDB](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (Point_expr, Point_expr)</td>
  <td>Gibt den Abstand zwischen den beiden GeoJSON Punkt Ausdrücken zurück.</td>
</tr>
<tr>
  <td>ST_WITHIN (Point_expr, Polygon_expr)</td>
  <td>Gibt einen booleschen Ausdruck, der angibt, ob der im ersten Argument angegebene GeoJSON Punkt innerhalb der GeoJSON Polygons in das zweite Argument ist.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Gibt einen booleschen Wert, der angibt, ob der angegebene GeoJSON Punkt oder Polygons Ausdruck gültig ist.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Gibt JSON-Werte, enthält ein boolescher Wert, wenn der angegebene GeoJSON Punkt oder Polygons Ausdruck gültig ist, und ungültige, darüber hinaus den Grund als Zeichenfolgenwert.</td>
</tr>
</table>

Räumliche Funktionen können auszuführenden Näherung Querries gegen raumgeometrischen Daten verwendet werden. Hier wird beispielsweise eine Abfrage, die alle familiäre Dokumente gibt, die innerhalb von 30 km von der angegebenen Position die integrierte ST_DISTANCE-Funktion verwendet werden. 

**Abfrage**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Wenn Sie die räumliche Indizierung in Ihrer Richtlinie Indizierung einbeziehen, werden "Abstand Abfragen" effizient über den Index bereitgestellt. Weitere Informationen zu räumliche Indizierung finden Sie weiter unten im Abschnitt. Wenn Sie einen räumlichen Index für die angegebenen Pfade besitzen, können Sie weiterhin raumbezogener Abfragen durchführen, indem Sie angeben `x-ms-documentdb-query-enable-scan` Anforderungsheader mit den Wert auf "True". In .NET können dazu werden optionale **FeedOptions** als Argument übergeben, um Abfragen mit [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) auf True festgelegt ist. 

Prüfen, ob ein Punkt innerhalb eines Polygons liegt, kann ST_WITHIN verwendet werden. Häufig verwendeten Polygone Grenzen wie Postleitzahlen, Bundesstaat Begrenzungen oder natürlich formationen darstellen. Erneut Wenn Sie die räumliche Indizierung in Ihrer Richtlinie Indizierung einbeziehen, werden dann "in" Abfragen effizient über den Index bereitgestellt werden. 

Polygon Argumente ST_WITHIN können enthalten nur ein einfaches klingeln, d. h. die Polygone müssen nicht Lücken in diese. Überprüfen Sie die [DocumentDB Grenzwerte](documentdb-limits.md) für die maximale Anzahl von Punkten in einem Polygon für eine Abfrage ST_WITHIN zulässig.

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Ähnlich wie nicht übereinstimmende Typen funktioniert in DocumentDB Abfrage, wenn in der Speicherortwert angegeben haben, entweder Argument ist falsch formatierte oder ist ungültig, und es ergeben, **nicht definierten** und ausgewerteten Dokument aus den Abfrageergebnissen übersprungen werden. Wenn Ihre Abfrage keine Ergebnisse zurückgibt, ausführen ST_ISVALIDDETAILED zum Debuggen, warum der Spatail ungültig ist.     

ST_ISVALID und ST_ISVALIDDETAILED kann verwendet werden, prüfen, ob ein räumliche Objekt gültig ist. Die folgende Abfrage überprüft beispielsweise die Gültigkeit eines Punkts mit einem Wert außerhalb des Gültigkeitsbereichs Breite (-132.8). ST_ISVALID gibt nur der boolesche Wert und ST_ISVALIDDETAILED zurück, der boolesche Wert und eine Zeichenfolge mit den Grund, warum es als ungültig angesehen wird.

**Abfrage**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Ergebnisse**

    [{
      "$1": false
    }]

Diese Funktionen können auch Polygone überprüft verwendet werden. Beispielsweise verwenden hier wir ST_ISVALIDDETAILED ein Polygons überprüft, das nicht abgeschlossen ist. 

**Abfrage**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Ergebnisse**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Die schließt die räumliche Funktionen und die SQL-Syntax für DocumentDB. Jetzt werfen Sie einen Blick auf wie LINQ Abfragen funktioniert und wie sie mit der Syntax interagiert wir bisher gesehen habe.

## <a name="linq-to-documentdb-sql"></a>LINQ to SQL DocumentDB
LINQ ist ein programming .NET-Modell, das Berechnung als Abfragen auf Streams von Objekten angibt. DocumentDB bietet eine Webclient Seite-Bibliothek auf der Benutzeroberfläche mit LINQ durch die Erleichterung einer Konvertierung zwischen JSON und .NET Objekte und eine Zuordnung aus einer Teilmenge von LINQ Abfragen zu DocumentDB Abfragen. 

Die nachstehende Abbildung zeigt die Architektur LINQ-Abfragen mithilfe von DocumentDB zu unterstützen.  Verwenden den DocumentDB Client, können Entwickler Objekt **IQueryable** erstellen, die den DocumentDB Abfrage-Anbieter, der wobei dann die LINQ-Abfrage in einer Abfrage DocumentDB übersetzt direkt in Abfragen. Die Abfrage wird dann auf dem Server DocumentDB zum Abrufen von Ergebnissen im JSON-Format übergeben. Die zurückgegebenen Ergebnisse werden in einem Stream .NET Objekte clientseitig deserialisiert.

![Architektur unterstützen LINQ Abfragen mithilfe von DocumentDB - SQL-Syntax, JSON-Abfragesprache, Datenbankkonzepte und SQL-Abfragen][1]
 


### <a name="net-and-json-mapping"></a>.NET und JSON-Zuordnung
Die Zuordnung von .NET Objekte und JSON-Dokumente ist natürlich – jedes Element Datenfeld ein JSON-Objekt, wobei der Feldname das Webpart "Key" des Objekts zugeordnet ist, und das Webpart "Wert" ist rekursiv den Wertteil des Objekts zugeordnet zugeordnet ist. Betrachten Sie das folgende Beispiel aus. Das Familie Objekt erstellt wird das JSON-Dokument zugeordnet, wie unten dargestellt. Und umgekehrt, das JSON-Dokument zurück zu einem .NET Objekt zugeordnet ist.

**C#-Klasse**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ zu SQL-Übersetzung
Der Abfrage-Anbieter DocumentDB führt eine optimale Leistung Zuordnung von einer LINQ-Abfrage in eine DocumentDB SQL-Abfrage aus. In der folgenden Beschreibung wird davon ausgegangen hat der Leser von LINQ vertraut sind.

Für das Typsystem unterstützen wir zunächst alle JSON Grundtypen – numerische Typen, Boolesch, Zeichenfolge und Null. Es werden nur diese JSON-Typen unterstützt. Die folgenden skalaren Ausdrücke werden unterstützt.

-   Konstante Werte – diese enthält konstante Werte von den Grunddatentypen zum Zeitpunkt die Abfrage ausgewertet wird.

-   Array-Eigenschaft Indexausdrücke – diese Ausdrücke finden Sie in der Eigenschaft eines Objekts oder eines Elements Matrix zurück.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Arithmetische Ausdrücke – diese allgemeine arithmetische Ausdrücke auf numerische und boolesche Werte einbeziehen. Eine vollständige Liste finden Sie in der SQL-Spezifikation.

        2 * family.children[0].grade;
        x + y;

-   Vergleich von Zeichenfolgenausdruck - diese Vergleich einen Zeichenfolgenwert einige Konstanten Zeichenfolgenwert einbeziehen.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   Objekt/Array Erstellungsausdruck - diese Ausdrücke Absenderadresse Objekt anonymen Typ oder zusammengesetzten Wert oder ein Array von solche Objekte. Diese Werte können geschachtelt werden.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Liste der unterstützten LINQ-Operatoren
Hier ist eine Liste der unterstützten LINQ Operatoren im Lieferumfang von DocumentDB .NET SDK LINQ-Anbieter.

-   **Wählen Sie aus**: Projektionen übersetzen Objektkonstruktion einschließlich SQL auswählen
-   **Wo**: Filter übersetzen in SQL WHERE und Unterstützung für die Übersetzung zwischen & &, || und! um die SQL-Operatoren
-   **SelectMany**: ermöglicht Entladung von Arrays der SQL JOIN-Klausel. Können verwendet werden, um Kette/mithilfe von Ausdrücken Filtern auf Arrayelemente verschachteln.
-   **OrderBy und OrderByDescending**: zu ORDER BY aufsteigend/absteigend übersetzt:
-   **CompareTo**: übersetzt zum Bereich Vergleiche sind möglich. Gängige für Zeichenfolgen, da diese nicht in .NET vergleichbaren befinden
-   **Optimieren**: übersetzt an den Anfang der SQL zum Einschränken der Ergebnisse einer Abfrage
-   **Mathematische Funktionen**: unterstützt Übersetzung aus. Netz des Abs, ARCCOS, ARCSIN, ARCTAN, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, melden Sie sich, Sin, Sqrt, Tan, Abschneiden zu der entsprechenden integrierten SQL-Funktionen.
-   **Zeichenfolgenfunktionen**: unterstützt Übersetzung aus. Netz des verketten, enthält, EndsWith IndexOf, Anzahl, ToLower, TrimStart, ersetzen, umgekehrte Richtung, TrimEnd, StartsWith, Teilzeichenfolge, ToUpper die entsprechende integrierten SQL-Funktionen.
-   **Matrixfunktionen**: unterstützt Übersetzung aus. Netz des verketten, enthält und zählen, um die entsprechenden integrierten SQL-Funktionen.
-   **Geodaten Erweiterungsfunktionen**: Übersetzung aus Stubmethoden Entfernung innerhalb IsValid und IsValidDetailed in den entsprechenden integrierten SQL-Funktionen unterstützt.
-   **Benutzerdefinierte Funktion Erweiterungsfunktion**: unterstützt Übersetzung aus der Stub-Methode UserDefinedFunctionProvider.Invoke in der entsprechenden Benutzer definiert (Funktion).
-   **Sonstiges**: Übersetzung des zusammengesetzten und bedingte Operatoren unterstützt. Können Sie übersetzen Zeichenfolge enthält, ARRAY_CONTAINS oder der SQL-IN, je nach Kontext enthält.

### <a name="sql-query-operators"></a>SQL-Abfrage-Operatoren
Hier sind einige Beispiele, die veranschaulichen, wie einige der standardmäßigen LINQ Abfrageoperatoren nach unten bis zum DocumentDB Abfragen übersetzt werden.

#### <a name="select-operator"></a>Wählen Sie Operator
Die Syntax ist `input.Select(x => f(x))`, wobei `f` ist ein Skalarausdruck.

**LINQ Lambda-Ausdruck**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ Lambda-Ausdruck**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ Lambda-Ausdruck**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany-operator
Die Syntax ist `input.SelectMany(x => f(x))`, wobei `f` ist ein Skalarausdruck, der einen Auflistungstyp zurückgibt.

**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Wo Operator
Die Syntax ist `input.Where(x => f(x))`, wobei `f` ist ein Skalarausdruck die booleschen Wert zurückgibt.

**LINQ Lambda-Ausdruck**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ Lambda-Ausdruck**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Zusammengesetzte SQL-Abfragen
Die oben aufgeführten Operatoren können leistungsfähigere Abfragen bilden kombiniert werden. Da DocumentDB geschachtelte Websitesammlungen unterstützt, kann die Zusammenstellung entweder verkettet oder geschachtelt werden.

#### <a name="concatenation"></a>Verkettung 

Die Syntax ist `input(.|.SelectMany())(.Select()|.Where())*`. Eine verkettete Abfrage mit einem optionalen beginnen kann `SelectMany` Abfrage gefolgt von mehreren `Select` oder `Where` Operatoren.


**LINQ Lambda-Ausdruck**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ Lambda-Ausdruck**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ Lambda-Ausdruck**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Das Verschachteln von

Die Syntax ist `input.SelectMany(x=>x.Q())` F Wo finde ich eine `Select`, `SelectMany`, oder `Where` Operator.

In einer geschachtelten Abfrage wird die innere Abfrage auf jedes Element der äußeren Auflistung angewendet. Ein wichtiges Feature besteht, dass die innere Abfrage auf die Felder der Elemente in der äußeren Auflistung wie verweisen kann Selbstjoins.

**LINQ Lambda-Ausdruck**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ Lambda-Ausdruck**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ Lambda-Ausdruck**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>Ausführen von SQL-Abfragen
DocumentDB macht Ressourcen über eine REST-API, die von einer beliebigen Lage Ausführen einer Anforderung HTTP-/HTTPS-Sprache aufgerufen werden können. Darüber hinaus bietet DocumentDB programming Bibliotheken für verschiedene gängige Sprachen wie .NET, Node.js, JavaScript und Python. Die REST-API und die verschiedenen Bibliotheken unterstützen bis SQL Abfragen. .NET SDK unterstützt LINQ Abfragen sowie SQL.

So erstellen Sie eine Abfrage, und senden Sie ihn mit einem DocumentDB Datenbank-Konto Sie in den folgenden Beispielen.

### <a name="rest-api"></a>REST-API
DocumentDB bietet ein Öffnen Rest programming Modell über HTTP. Datenbankkonten können mithilfe eines Azure-Abonnements bereitgestellt werden. Das DocumentDB Ressourcenmodell besteht aus einem Sätze von Ressourcen unter einer Datenbankkonto, von denen jede mit einem URI logische und unveränderliche adressiert ist. Eine Reihe von Ressourcen ist ein Feed in diesem Dokument genannt. Ein Datenbankkonto besteht aus einer Reihe von Datenbanken mit jeweils mehrere Websitesammlungen, jede welche aktivieren Dokumente, UDFs und andere Ressourcentypen enthalten.

Das Modell grundlegende Interaktion mit den folgenden Ressourcen ist durch die HTTP-Verben GET, sich, bereitstellen und Löschen mit ihre standard Interpretation. Das Beitrag Verb wird für die Erstellung einer neuen Ressource, für die Ausführung einer gespeicherten Prozedur oder für die Ausgabe einer Abfrage DocumentDB verwendet. Abfragen werden immer nur Vorgänge mit keine Seite-Effekte gelesen.

Die folgenden Beispiele zeigen einen Beitrag für eine DocumentDB Abfrage anhand einer Websitesammlung mit den zwei Stichproben Dokumenten vorgenommen, dass wir bisher überprüft haben. Die Abfrage enthält einen einfachen Filter auf das JSON-Name-Eigenschaft. Beachten Sie die Verwendung von der `x-ms-documentdb-isquery` und Inhaltstyp: `application/query+json` Überschriften, um anzugeben, dass der Vorgang eine Abfrage ist.


**Anfordern**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Ergebnisse**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Das zweite Beispiel zeigt eine komplexe Abfrage, die mehrere Ergebnisse aus der Verknüpfung zurückgibt.

**Anfordern**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Ergebnisse**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Wenn den Ergebnissen einer Abfrage in einer einzelnen Seite Ergebnissen passt nicht, und klicken Sie dann die REST-API ein Fortsetzungstoken durch gibt die `x-ms-continuation-token` Antwort Kopfzeile. Clients können Ergebnisse nach einschließlich der Überschriften in nachfolgenden Ergebnisse neu. Die Anzahl der Ergebnisse pro Seite kann auch gesteuert werden, bis die `x-ms-max-item-count` Zahl Kopfzeile.

Verwenden Sie zum Verwalten der Daten Konsistenz-Richtlinie für Abfragen die `x-ms-consistency-level` Kopfzeile wie alle übrigen API Anfragen. Für Session-Konsistenz ist es erforderlich, als auch die neuesten echo an `x-ms-session-token` Cookie-Header in der Anforderung der Abfrage. Beachten Sie, dass die abgefragte Sammlung Indizierung Richtlinie auch die Konsistenz der Abfrageergebnisse beeinflussen kann. Mit Richtlinieneinstellungen Indizierung standardmäßig für Websitesammlungen der Index ist immer aktuell mit den Inhalt des Dokuments und Abfrageergebnisse werden die Konsistenz für Daten ausgewählt wurde, entsprechen. Wenn die Indizierung Richtlinie zu Lazy gelockert ist, können Abfragen veraltete Ergebnisse zurück. Weitere Informationen finden Sie in [DocumentDB Konsistenz Ebenen] [consistency-levels].

Wenn die Indizierung konfigurierte Richtlinie auf die Auflistung die angegebene Abfrage nicht unterstützt, gibt der DocumentDB Server 400 "Ungültige Anforderung" aus. Dies ist für Bereichsabfragen auf Pfade konfiguriert für Suchvorgänge Hash (Gleichheit) und Indizierung explizit ausgeschlossene Pfade zurückgegeben. Die `x-ms-documentdb-query-enable-scan` angegebene Header an, damit die Abfrage zu einen Scan ausführen, wenn Sie ein Index nicht verfügbar ist.

### <a name="c-net-sdk"></a>SDK C# (.NET)
.NET SDK unterstützt sowohl LINQ und SQL Abfragen. Im folgenden Beispiel wird gezeigt, wie die einfacher Filterabfrage eingeführt werden oben in diesem Dokument durchgeführt.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


In diesem Beispiel werden zwei Eigenschaften auf Übereinstimmung in jedem Dokument verglichen und anonyme Projektionen verwendet. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Im nächste Beispiel zeigt die Verknüpfungen, die über LINQ SelectMany ausgedrückt.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Der Client .NET durchläuft automatisch auf allen Seiten des Abfrageergebnisse in der Foreach Blöcke wie oben gezeigt. Die Abfrageoptionen eingeführt werden im Abschnitt REST-API stehen auch in der .NET SDK mit den `FeedOptions` und `FeedResponse` Klassen in der Methode CreateDocumentQuery. Die Anzahl der Seiten mit gesteuert werden kann die `MaxItemCount` Einstellung. 

Sie können auch explizit steuern Seitennavigation erstellen `IDocumentQueryable` mithilfe der `IQueryable` Objekt, dann nach Lesen der` ResponseContinuationToken` Werte und übergibt diese als Hintergrund `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery`kann festgelegt werden, um scannt zu aktivieren, wenn die Abfrage durch die Indizierung konfigurierte Richtlinie nicht unterstützt werden kann. Für partitionierte Websitesammlungen, können Sie `PartitionKey` zum Ausführen der Abfrage für ein einzelnes partitionieren (obwohl DocumentDB automatisch diese aus den Abfragetext extrahiert werden kann), und `EnableCrossPartitionQuery` um Abfragen auszuführen, die möglicherweise mehrere Partitionen ausgeführt werden müssen. 

Weitere Beispiele mit Abfragen finden Sie unter [DocumentDB .NET Beispiele](https://github.com/Azure/azure-documentdb-net) . 

### <a name="javascript-server-side-api"></a>JavaScript serverseitigen-API 
DocumentDB bietet ein programming Modell für die Ausführung von JavaScript-basierte Anwendungslogik direkt auf die Sammlungen mit gespeicherten Prozeduren und Triggern. JavaScript-Logik registriert Ebene einer Websitesammlung kann dann Datenbankvorgänge für die Vorgänge an den Dokumenten der angegebenen Websitesammlung ausgeben. Diese Vorgänge werden in umgebende ACID-Transaktionen eingeschlossen.

Im folgende Beispiel zeigen, wie Sie die QueryDocuments in die JavaScript-Server-API zu verwenden, um Abfragen aus zu gestalten innen gespeicherte Prozeduren und Trigger.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Aggregatfunktionen

Systemeigene Unterstützung für Aggregatfunktionen im Works ist, aber wenn Sie in der Zwischenzeit zählen oder summieren Funktionalität benötigen, können Sie das gleiche Ergebnis mithilfe verschiedene Methoden erzielen.  

Auf finden Sie hier:

- Sie können Aggregatfunktionen ausführen, indem Sie beim Abrufen der Daten und Anzahl lokal ausführen. Verwenden Sie eine effizient Abfrageprojektion wie empfohlen hat `SELECT VALUE 1` anstatt vollständigen Dokument wie `SELECT * FROM c`. Dadurch wird die Anzahl der Dokumente, die auf jeder Seite der Ergebnisse, vermeiden zusätzliche Schleifen zum Dienst damit auch bei Bedarf verarbeitet maximieren.
- Sie können auch eine gespeicherte Prozedur um zu minimieren Netzwerkwartezeit bei wiederholten einer Schleife verwenden. Ein Beispiel der gespeicherten Prozedur, die die Anzahl die für eine Filterabfrage angegebenen berechnet finden Sie unter [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Die gespeicherte Prozedur kann Benutzer zum Kombinieren von umfangreichen Geschäftslogik zusammen mit Aggregationen in eine effiziente Möglichkeit ausführen aktivieren.

Klicken Sie auf schreiben Pfad:

- Eine andere allgemeine Muster besteht darin, die Ergebnisse in den Pfad "Schreiben" vorab aggregieren. Dies ist besonders attraktive, wenn die Lautstärke der Anfragen "Lesen" größer als die der Anfragen "Schreiben" befindet. Einmal vorab aggregiert, die Ergebnisse mit einem einzigen Punkt leseanforderungen verfügbar sind.  In DocumentDB vorab aggregieren am besten zum Einrichten eines Triggers, das mit jedem "Schreiben" aufgerufen wird, und aktualisieren ein Metadatendokument, die die neuesten Ergebnisse für die Abfrage enthält, die gerade materialisiert wird. Beispielsweise finden Sie in der Stichprobe [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) , in dem die MinSize, MaxSize und TotalSize des Dokuments Metadaten für die Websitesammlung aktualisiert. Die Stichprobe kann erweitert werden, um einen Indikator, Summe usw. zu aktualisieren.

##<a name="references"></a>Verweise
1.  [Einführung in Azure DocumentDB][introduction]
2.  [DocumentDB SQL-Spezifikation](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [Beispiele für .NET DocumentDB](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB Konsistenz Ebenen][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON- [http://json.org/](http://json.org/)
7.  JavaScript-Spezifikation [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Abfrage Auswertung Techniken für große Datenbanken [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Verarbeiten von Abfragen in parallelen relationalen Datenbanksysteme, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, Verarbeiten von Abfragen in parallelen relationalen Datenbanksysteme, IEEE Computer Society Press, 1994.
12. Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Schwein Lateinisch: eine nicht, damit Fremdschlüssel Sprache für die Datenverarbeitung, SIGMOD 2008.
13.     G. Graefe. Die überlappend Framework zur Optimierung der Abfrage. Eng IEEE-Daten Bull., 18 Absatz 3: 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
