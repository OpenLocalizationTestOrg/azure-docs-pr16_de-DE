<properties
    pageTitle="Beispiele für häufige Verwendungsmuster in Stream Analytics Abfrage | Microsoft Azure"
    description="Allgemeine Azure Stream Analytics Abfrage Mustern "
    keywords="Beispiele für die Abfrage"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Beispiele für allgemeine Stream Analytics Verwendungsmustern Abfragen

## <a name="introduction"></a>Einführung

Abfragen in Azure Stream Analytics werden in einer SQL-ähnliche Abfragesprache ausgedrückt die des [Streams Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx) Referenzhandbuchs beschrieben ist.  In diesem Artikel werden die Lösungen für verschiedene gängige Abfragemuster basierend auf realen Szenarien.  Es wird eine laufende Arbeit und werden weiterhin mit neuen Mustern kontinuierlich aktualisiert werden.

## <a name="query-example-data-type-conversions"></a>Beispiel für eine Abfrage: von Datentypen
**Beschreibung**: die Typen der Eigenschaften für die Eingabewerte Stream definieren.
Beispielsweise Auto Stärke für die Eingabewerte Stream bald als Zeichenfolgen und muss in INT ausführen konvertiert ADDIEREN nach oben.

**Eingabe**:

| Stellen Sie | Zeit | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Ergebnis**:

| Stellen Sie | Gewicht |
| --- | --- |
| Honda | 3000 |

**Lösung**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Erläuterung**: Verwenden Sie eine CAST-Anweisung für das Feld Stärke zur Angabe des Typs (finden Sie in der Liste der unterstützten Datentypen [hier](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Beispiel für eine Abfrage: mit gefällt mir/nicht gerne übereinstimmenden Mustern
**Beschreibung**: Überprüfen Sie, dass der Wert eines Felds auf das Ereignis einem bestimmten Muster beispielsweise Kennzeichen der Rückgabetyp entspricht, die mit A beginnen und enden mit 9

**Eingabe**:

| Stellen Sie | LicensePlate | Zeit |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Ergebnis**:

| Stellen Sie | LicensePlate | Zeit |
| --- | --- | --- |
| Toyota | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Lösung**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Erläuterung**: Verwenden Sie die wie Anweisung zu um überprüfen, ob der Wert des Felds LicensePlate beginnt mit A und verfügt über eine beliebige Zeichenfolge mit 0 (null) oder mehr Zeichen und mit 9 endet. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Beispiel für eine Abfrage: Geben Logik für unterschiedliche Fällen/Werte (CASE-Anweisungen)
**Beschreibung**: Bereitstellen von anderen Berechnung für ein Feld anhand einiger Kriterien.
Beispielsweise bieten Sie einen beschreibenden Text für wie viele Autos eines bestimmten Herstellers mit besonderen Fall für 1 zu übergeben.

**Eingabe**:

| Stellen Sie | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Ergebnis**:

| CarsPassed | Zeit |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01-01T00:00:10.0000000Z |

**Lösung**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Erläuterung**: der Groß-/KLEINSCHREIBUNG-Klausel können wir eine andere Berechnung anhand einiger Kriterien (in unserem Fall die Anzahl von Autos im Fenster aggregieren) bereitstellen.

## <a name="query-example-send-data-to-multiple-outputs"></a>Beispiel für eine Abfrage: Senden von Daten an mehrere Ausgaben
**Beschreibung**: Senden von Daten an mehreren Ausgabe Ziele in einem Auftrag.
Angenommen, Analysieren von Daten für eine Warnung Schwellenwert-basierten und archivieren alle Ereignisse zu Blob-Speicher

**Eingabe**:

| Stellen Sie | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Stellen Sie | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Stellen Sie | Zeit | Zählen |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Lösung**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Erläuterung**: in der Klausel teilt Stream Analytics welche die Ausgaben zum Schreiben von Daten aus dieser Anweisung.
Die erste Abfrage ist eine Pass-Through-Daten, die wir, dass wir ArchiveOutput mit dem Namen der Ausgabe erhalten haben.
In der zweiten Abfrage enthält einige einfache Aggregation und Filtern und sendet die Ergebnisse an einen untergeordneten Warnsystem.
*Hinweis*: Sie können auch Resultate CTEs (d. h. mit Anweisungen) in mehrere Ausgabe Anweisungen wiederverwenden – Dies hat den Vorteil weniger Leser mit der Datenquelle zu öffnen.
Beispielsweise 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Beispiel für eine Abfrage: eindeutige Werte zu zählen
**Beschreibung**: zählen der Anzahl eindeutiger Feldwerte, die in den Stream in einem Zeitfenster angezeigt werden.
Wie viele eindeutige Erstellen von Autos beispielsweise über die weder eine gebührenpflichtige Booth in einem zweiten 2 Fenster übergeben?

**Eingabe**:

| Stellen Sie | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Ergebnis:**

| Zählen | Zeit |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Lösung:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Erläuterung:** Wir führen Sie eine anfängliche Aggregation um eindeutig ist, mit deren Anzahl über das Fenster zu erhalten.
Wir führen Sie dann eine Aggregation von wie viele wir alle eindeutigen Werte in einem Fenster erhalten denselben Zeitstempel, und klicken Sie dann das zweite Aggregation Fenster 2 Windows nicht aus dem ersten Schritt aggregieren minimal sein muss angegeben haben – ist.

## <a name="query-example-determine-if-a-value-has-changed"></a>Beispiel für eine Abfrage: feststellen, ob ein Wert geändert wurde#
**Beschreibung**: schauen Sie sich einen vorherigen Wert, um festzustellen, ob er unterscheidet sich von der aktuelle Wert der vorherigen Auto unterwegs weder eine gebührenpflichtige beträgt beispielsweise den gleichen als das aktuelle Auto zu verwenden?

**Eingabe**:

| Stellen Sie | Zeit |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Ergebnis**:

| Stellen Sie | Zeit |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Lösung**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Erläuterung**: verwenden POSITIVEN in die Eingabe einsehen streamen wieder ein Ereignis, und rufen Sie den Wert erstellen. Klicken Sie dann vergleichen Sie ihn mit den Stellen auf das aktuelle Ereignis und ausgeben Sie das Ereignis, wenn sie sich unterscheiden.

## <a name="query-example-find-first-event-in-a-window"></a>Beispiel für eine Abfrage: Suchen ersten Ereignisses in einem Fenster
**Beschreibung**: Suchen erstes Auto in jedem Intervall 10 Minuten?

**Eingabe**:

| LicensePlate | Stellen Sie | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Ergebnis**:

| LicensePlate | Stellen Sie | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Lösung**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Jetzt wir ändern das Problem und suchen erstes Auto bestimmten Stellen in jedem Intervall 10 Minuten.

| LicensePlate | Stellen Sie | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Lösung**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Beispiel für eine Abfrage: Suchen letzten Ereignisses in einem Fenster
**Beschreibung**: Suchen letzten Auto in jedem Intervall 10 Minuten.

**Eingabe**:

| LicensePlate | Stellen Sie | Zeit |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Ergebnis**:

| LicensePlate | Stellen Sie | Zeit |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Lösung**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Erläuterung**: umfasst zwei Schritte in der Abfrage – der ersten Phase findet neuesten Zeitstempel in Windows 10 Minuten. Im zweite Schritt verknüpft die Ergebnisse der ersten Abfrage mit dem ursprünglichen Stream zum Suchen von Ereignissen übereinstimmenden letzten Zeitstempel in jedem Fenster. 

## <a name="query-example-detect-the-absence-of-events"></a>Beispiel für eine Abfrage: erkennen, die Abwesenheit von Ereignissen
**Beschreibung**: Überprüfen Sie, dass ein Stream keinen Wert aufweist, die bestimmten Kriterien entspricht.
Beispielsweise eingegeben 2 aufeinander folgenden Autos aus bestimmten Herstellers weder eine gebührenpflichtige ständige innerhalb von 90 Sekunden?

**Eingabe**:

| Stellen Sie | LicensePlate | Zeit |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA-999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | 987-QUALITÄT | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI-345 | 2015-01-01T00:00:04.0000000Z |

**Ergebnis**:

| Stellen Sie | Zeit | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA-999 | ABC-123 | 2015-01-01T00:00:01.0000000Z |

**Lösung**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Erläuterung**: verwenden POSITIVEN in die Eingabe einsehen streamen wieder ein Ereignis, und rufen Sie den Wert erstellen. Klicken Sie dann vergleichen Sie ihn mit den Stellen auf das aktuelle Ereignis und ausgeben Sie das Ereignis, wenn sie gleich sind und POSITIVEN zum Abrufen von Daten über das vorherige Auto verwenden.

## <a name="query-example-detect-duration-between-events"></a>Beispiel für eine Abfrage: Erkennen von Dauer zwischen Ereignissen
**Beschreibung**: Suchen nach der Dauer eines bestimmten Ereignisses. Bestimmen Sie, beispielsweise aufgrund einer Web Clickstream Zeit für eine Funktion.

**Eingabe**:  
  
| Benutzer | Feature | Ereignis | Zeit |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Starten | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Ende | 2015-01-01T00:00:08.0000000Z |
  
**Ergebnis**:  
  
| Benutzer | Feature | Dauer |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Lösung**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Erläuterung**: letzte Funktion Verwendung zum letzten Uhrzeitwert abrufen, wenn Ereignistyp war 'Start'. Beachten Sie, dass letzten Funktion verwendet PARTITION BY [Benutzer], um anzugeben, dass das Ergebnis pro Benutzer eindeutige berechnet werden.  Die Abfrage weist einen 1 Stunde maximalen Schwellenwert für zeitlichen Unterschied zwischen "Start" und "Beenden" Ereignisse aber lässt sich als erforderlich (Grenzwert DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Beispiel für eine Abfrage: Erkennen von Dauer einer Bedingung
**Beschreibung**: finden Sie heraus, die wie lange eine Bedingung für aufgetreten ist.
Angenommen, die einen Fehler, der alle Autos gibt es eine falsche Stärke geführt haben (über 20.000 Pfund) – wir die Dauer des Fehlers berechnen möchten.

**Eingabe**:

| Stellen Sie | Zeit | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Ergebnis**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Lösung**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Erläuterung**: verwenden POSITIVEN zum Anzeigen von Eingabe-Streams für 24 Stunden, und suchen Sie nach Instanzen gesucht, in dem StartFault und StopFault über erstreckt, Gewicht < 20000.

## <a name="query-example-fill-missing-values"></a>Beispiel für eine Abfrage: Ausfüllen der fehlenden Werte
**Beschreibung**: für den Stream von Ereignissen, die fehlende Werte enthalten, Naturprodukte einen Stream von Ereignissen mit regelmäßigen Abständen.
Beispielsweise generieren Sie Ereignis alle 5 Sekunden, die den am häufigsten zuletzt Sicht-Datenpunkt meldet, an.

**Eingabe**:

| t | Wert |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

**Ausgeben (ersten 10 Zeilen)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Lösung**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Erläuterung**: Diese Abfrage generiert Ereignisse pro Sekunde 5 und das letzte Ereignis, bevor empfangen wurde, ausgegeben wird. [Nach dem Frequenzsprungverfahren Fenster] (https://msdn.microsoft.com/library/dn835041.aspx "Hopping Fenster - Azure Stream Analytics") Dauer bestimmt, wie weit zurück, die die Abfrage aussehen wird, um das neueste Ereignis (300 Sekunden in diesem Beispiel) zu finden.


## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
