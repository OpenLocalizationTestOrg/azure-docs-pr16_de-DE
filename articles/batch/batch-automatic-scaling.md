<properties
    pageTitle="Automatisch berechnen Maßstab Knoten in einem Stapel Azure Pool | Microsoft Azure"
    description="Aktivieren der automatischen Skalierung auf einem Ressourcenpool Cloud Die Anzahl der berechnen Knoten im Pool dynamisch angepasst."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Automatisch skalieren Knoten in einem Stapel Azure Pool berechnen

Mit der automatischen Skalierung, kann die Stapel Azure Service dynamisch hinzufügen oder Entfernen von Datenverarbeitungsknoten in einem Ressourcenpool auf Grundlage von Parametern, die Sie definieren. Sie können sowohl Zeit und Geld potenziell sparen, indem automatisch anpassen des Betrags berechnen Leistungsfähigkeit von der Anwendung – verwendeten Knoten des Projekts Vorgang Abnahme langsam hinzufügen und entfernen, wenn diese verkleinern.

Aktivieren der automatischen Skalierung auf einem Ressourcenpool von Computeknoten durch Verknüpfen mit einer *Formel automatisch skalieren* , die Sie, wie definieren Sie mit der [PoolOperations.EnableAutoScale] [ net_enableautoscale] Methode in der Bibliothek [Stapel .NET](batch-dotnet-get-started.md) . Der Dienst Stapel verwendet die folgende Formel klicken Sie dann die Anzahl von Computeknoten bestimmt, die zum Ausführen Ihrer Arbeitsbelastung erforderlich sind. Stapel reagiert auf Kriterien Service Beispieldaten, die in regelmäßigen Abständen erfasst werden, und passt die Anzahl von Computeknoten im Pool eine konfigurierbare Intervall auf Grundlage Ihrer Formel.

Sie können die automatische Skalierung, wenn ein Ressourcenpool erstellt wird, oder auf einem vorhandenen Pool aktivieren. Sie können auch eine vorhandene Formel auf einem Ressourcenpool ändern, die "automatisch skalieren" aktiviert ist. Stapel bietet die Möglichkeit, Formeln auswerten, bevor Sie Pools zuweisen sowie überwachen den Status der automatischen Skalierung ausgeführt.

## <a name="automatic-scaling-formulas"></a>Automatische Anpassungsbereich für Formeln

Eine automatische Skalierung Formel ist eine Zeichenfolge, die Sie definieren, die eine oder mehrere Anweisungen enthält, und einem Ressourcenpool [AutoScaleFormula] zugeordnet ist[ rest_autoscaleformula] Element (Stapel REST) oder [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] Eigenschaft (Stapel .NET). Bei einem Ressourcenpool zugewiesen, verwendet Stapel Service die Formel, um die Ziel-Anzahl von Computeknoten im Pool für nächsten Intervall des Verarbeitung (mehr Intervallen später) zu bestimmen. Die Formel Zeichenfolge darf nicht 8 KB Größe überschreiten, beziehen Sie können bis zu 100 Anweisungen, die durch Semikolons getrennt werden, und können Zeilenumbrüche und Kommentare enthalten.

Sie können über automatische Anpassungsbereich für Formeln als mit einem Stapel automatisch skalieren "Sprache". vorstellen. Formel Aussagen sind frei formatierte Ausdrücke, die sowohl Dienst definiert Variablen (Variablen vom Dienst Stapel definiert) und benutzerdefinierte Variablen (Variablen, die Sie definieren) enthalten sein können. Sie können verschiedene Vorgänge auf diese Werte mithilfe der integrierten Typen, Operatoren und Funktionen ausführen. Eine Anweisung kann beispielsweise das folgende Formular in Anspruch nehmen:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formeln in der Regel mehrere Anweisungen, die Operationen in vorherigen Anweisungen auf Werte, die abgerufen werden. Angenommen, wir rufen Sie zunächst einen Wert für `variable1`, übergeben Sie ihn an eine Funktion zum Auffüllen `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Mit den folgenden Anweisungen in die Formel, gewünscht ist eine Reihe von Computeknoten eintreffen, der Pool das – skaliert werden sollte, das **Ziel** Anzahl **dedizierten Knoten**. Diese Nummer möglicherweise höhere, klein und die aktuelle Anzahl der Knoten im Pool entspricht. Stapel wertet eine Ressourcenpool automatisch skalieren Formel in bestimmten Intervallen ([Automatische Skalierung Intervalle](#automatic-scaling-interval) werden siehe unten). Klicken Sie dann wird es die Ziel-Anzahl der Knoten im Pool der Anzahl anpassen, die die Formel automatisch skalieren zum Zeitpunkt der Auswertung angibt.

Als ein kurzes Beispiel gibt diese Formel zwei Zeilen automatisch skalieren, dass die Anzahl der Knoten entsprechend der Anzahl der aktiven Aufgaben, bis zu einem Maximum von 10 Computeknoten angepasst werden sollte:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Die nächsten Abschnitten dieses Artikels erläutern die verschiedenen Elemente, aus denen automatisch skalieren Formeln, einschließlich Variablen, Operatoren, Vorgänge und Funktionen besteht. Finden Sie heraus, wie verschiedene berechnen Ressourcen- und Aufgabendaten Kennzahlen in Stapel erhalten. Diese Kennzahlen können Sie Ihre Ressourcenpool des Knoten zählen basierend auf Ressource: Einsatz und Vorgangsstatus intelligente anpassen. Klicken Sie dann erfahren Sie, wie Sie eine Formel erstellen und Aktivieren der automatischen Skalierung auf einem Ressourcenpool mithilfe des Stapels REST und die APIs der .NET. Wir werden mit wenigen Beispielformeln abzuschließen.

> [AZURE.IMPORTANT] Jedes Blatt Azure-Konto ist auf eine maximale Anzahl von Kernen (und daher Datenverarbeitungsknoten), die für die Verarbeitung verwendet werden kann. Der Stapel-Dienst wird nur auf diese Einschränkung Core Knoten erstellen. Daher möglicherweise nicht die Ziel-Anzahl von Computeknoten erreichen, die von einer Formel angegeben ist. Informationen zum Anzeigen und Ihr Konto Kontingente zunehmender finden Sie unter [Kontingente und Grenzwerte für den Stapel Azure-Dienst](batch-quota-limit.md) .

## <a name="variables"></a>Variablen

Sie können sowohl **Dienst definiert** und **benutzerdefinierte** Variablen in Formeln automatisch skalieren verwenden. Der Dienst definiert Variablen in den Stapel-Dienst integriert sind einige Lese-und Schreibzugriff, und einige sind schreibgeschützt. Benutzerdefinierte Elemente werden Variablen, den *Sie* festlegen. In der oben genannten zwei Zeilen Beispielformel `$TargetDedicated` ist ein Dienst defined Variable, während `$averageActiveTaskCount` ist eine benutzerdefinierte Variable.

Die folgenden Tabellen anzeigen Lese-und Schreibzugriff und schreibgeschützte Variablen, die vom Dienst Stapel definiert sind.

Sie können **Abrufen** und **festlegen** der Werte dieser Dienst definiert Variablen zur Verwaltung der Anzahl von Computeknoten in einem Ressourcenpool:

| Lese-und Schreibzugriff Dienst definiert Variablen | Beschreibung |
| --- | --- |
| $TargetDedicated | Die Anzahl der **Ziel** der für den Pool **dedizierter Datenverarbeitungsknoten** . Dies ist die Anzahl der Knoten berechnen, denen das der Pool skaliert werden soll. Es ist eine Zahl "Target", da es für einen Pool nicht in der Zielliste Anzahl Knoten erreichen möglich ist. Dies kann auftreten, wenn die Ziel-Anzahl der Knoten durch eine nachfolgende automatisch skalieren Bewertung erneut geändert wird, vor der Pool die ursprüngliche Ziele erreicht hat. Sie können auch geschehen, wenn ein Stapel Konto-Knoten oder Core Kontingent erreicht wird, bevor die Ziel-Anzahl der Knoten erreicht ist. |
| $NodeDeallocationOption | Die Aktion, die bei Datenverarbeitungsknoten aus einem Ressourcenpool entfernt werden. Mögliche Werte sind:<ul><li>**Requeue**– Aufgaben sofort beendet und wieder in der Warteschlange verschoben, sodass sie nicht neu geplant werden.<li>**Beenden**: beendet Aufgaben sofort und werden aus der Warteschlange entfernt.<li>**Taskcompletion**– wartet zurzeit ausgeführt beschrieben vor, um Fertig stellen und dann den Knoten aus dem Pool entfernt.<li>**Retaineddata**– wartet alle lokalen Vorgang beibehalten Daten auf dem Knoten bereinigt werden, bevor Sie den Knoten aus dem Pool entfernen.</ul> |

Sie können den Wert dieser Dienst definiert Variablen so ändern, die auf Metrik vom Dienst Stapel basieren **erhalten** :

| Schreibgeschützte Dienst definiert Variablen | Beschreibung |
| --- | --- |
| $CPUPercent | Der Mittelwert Prozentsatz der CPU-Auslastung. |
| $WallClockSeconds | Die Anzahl von Sekunden verbraucht. |
| $MemoryBytes | Durchschnittliche Anzahl der Megabyte verwendet. |
| $DiskBytes | Durchschnittliche Anzahl der Gigabyte verwendet auf den lokalen Festplatten. |
| $DiskReadBytes | Die Anzahl von Bytes lesen. |
| $DiskWriteBytes | Die Anzahl von Bytes geschrieben. |
| $DiskReadOps | Die Anzahl von Vorgängen finden Sie hier Datenträger, die ausgeführt werden soll. |
| $DiskWriteOps | Die Anzahl der Datenträger Vorgänge ausgeführt. |
| $NetworkInBytes | Die Anzahl der eingehenden Bytes. |
| $NetworkOutBytes | Die Anzahl der ausgehende Bytes. |
| $SampleNodeCount | Die Anzahl der Knoten berechnen. |
| $ActiveTasks | Die Anzahl von Vorgängen in einem aktiven Zustand. |
| $RunningTasks | Die Anzahl von Vorgängen in einem laufenden Zustand. |
| $PendingTasks | Die Summe der $ActiveTasks und $RunningTasks. |
| $SucceededTasks | Die Anzahl der Aufgaben, die erfolgreich abgeschlossen. |
| $FailedTasks | Die Anzahl der Aufgaben, die fehlgeschlagen ist. |
| $CurrentDedicated | Die aktuelle Anzahl der dedizierten Knoten zu berechnen. |

> [AZURE.TIP] Die schreibgeschützt, Dienst definiert Variablen, die über angezeigt werden sind *Objekte* , die verschiedene Methoden zum Zugriff auf Daten, die jeweils zugeordneten bereitstellen. Weitere Informationen finden Sie in der nachfolgenden [Beispieldaten beziehen](#getsampledata) .

## <a name="types"></a>Datentypen

Diese **Typen** werden in einer Formel unterstützt.

- Doppelte
- doubleVec
- doubleVecList
- Zeichenfolge
- Zeitstempel – ist Zeitstempel eines zusammengesetzten Struktur, die die folgenden Elemente enthält:

    - Jahr
    - Monat (1 bis 12)
    - Tag (1-31)
    - Weekday (im Format Zahl, z. B. 1 für Montag)
    - Stunde (im 24-Stunden-Zahlenformat, z. B. 13 bedeutet 1 PM)
    - Minute (00-59)
    - Sekunde (00-59)
- TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Vorgänge

Diese **Vorgänge** sind auf den oben aufgelisteten Typen zulässig.

| Vorgang                             | Unterstützte Operatoren   | Ergebnistyp   |
| ------------------------------------- | --------------------- | ------------- |
| Doppelte doppelte *operator*              | +, -, *, /            | Doppelte            |
| doppelte *Operator* timeinterval        | *                     | TimeInterval      |
| DoubleVec *Operator* double           | +, -, *, /            | doubleVec         |
| DoubleVec *Operator* doubleVec        | +, -, *, /            | doubleVec         |
| TimeInterval *Operator* double        | *, /                  | TimeInterval      |
| TimeInterval *Operator* timeinterval  | +, -                  | TimeInterval      |
| TimeInterval *Operator* Zeitstempel     | +                     | Zeitstempel         |
| Timestamp *Operator* timeinterval     | +                     | Zeitstempel         |
| Timestamp *Operator* Zeitstempel        | -                     | TimeInterval      |
| *Operator*double                      | -, !                  | Doppelte            |
| *Operator*timeinterval                | -                     | TimeInterval      |
| Doppelte doppelte *operator*              | <, < =, ==, > =, >,! =  | Doppelte            |
| Zeichenfolge *Operator* Zeichenfolge              | <, < =, ==, > =, >,! =  | Doppelte            |
| Timestamp *Operator* Zeitstempel        | <, < =, ==, > =, >,! =  | Doppelte            |
| TimeInterval *Operator* timeinterval  | <, < =, ==, > =, >,! =  | Doppelte            |
| Doppelte doppelte *operator*              | & &, & #124; & #124;      | Doppelte            |

Wenn einen Double mit einem ternärer Operator testen (`double ? statement1 : statement2`), ungleich NULL ist **Wahr**und 0 (null) ist **falsch**.

## <a name="functions"></a>Funktionen

Diese vordefinierten **Funktionen** stehen für Ihre Verwendung in einer automatischen Skalierung Formel definieren.

| (Funktion)                          | Rückgabetyp   | Beschreibung
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | Doppelte        | Gibt den Mittelwert der mittlere Wert für alle Werte in der DoubleVecList an.
| Len(doubleVecList)                | Doppelte        | Gibt die Gesamtlänge der Vektorversion, die von der DoubleVecList erstellt wird.
| LG(Double)                        | Doppelte        | Gibt das Protokoll der doppelten Basis 2 zurück.
| LG(doubleVecList)                 | doubleVec     | Gibt das componentwise Protokoll Basis 2 von der DoubleVecList an. Eine vec(double) muss explizit für den Parameter übergeben werden. Andernfalls wird die Version doppelte lg(double) angenommen.
| LN(Double)                        | Doppelte        | Gibt der natürliche Logarithmus von der Double-Wert.
| LN(doubleVecList)                 | doubleVec     | Gibt das componentwise Protokoll Basis 2 von der DoubleVecList an. Eine vec(double) muss explizit für den Parameter übergeben werden. Andernfalls wird die Version doppelte lg(double) angenommen.
| Log(Double)                       | Doppelte        | Gibt das Protokoll der doppelten zur Basis 10 zurück.
| Log(doubleVecList)                | doubleVec     | Gibt das componentwise Protokoll von der DoubleVecList zur Basis 10 zurück. Eine vec(double) muss explizit für die einzelnen double-Parameter übergeben werden. Andernfalls wird die Version doppelte log(double) angenommen.
| Max(doubleVecList)                | Doppelte        | Gibt den größten Wert in der DoubleVecList an.
| Min(doubleVecList)                | Doppelte        | Gibt den kleinsten Wert in der DoubleVecList an.
| Norm(doubleVecList)               | Doppelte        | Gibt die normale zwei der Vektorversion, die von der DoubleVecList erstellt wird.
| Quantil (V DoubleVec, doppelte p) | Doppelte        | Gibt das Element Quantil der Vektor V zurück.
| Zufallszahl)                            | Doppelte        | Gibt einen Wert Zufallszahlen zwischen 0,0 und 1,0.
| Range(doubleVecList)              | Doppelte        | Gibt die Differenz zwischen dem min und max Werte in der DoubleVecList an.
| Std(doubleVecList)                | Doppelte        | Gibt die Standardabweichung der Stichprobe der Werte in der DoubleVecList an.
| Stop()                            |               | Beendet die Auswertung des Ausdrucks automatische Skalierung.
| SUM(doubleVecList)                | Doppelte        | Gibt die Summe aller Komponenten von der DoubleVecList an.
| Uhrzeit (DateTime Zeichenfolge = "")          | Zeitstempel     | Gibt den Zeitstempel der aktuellen Uhrzeit, wenn keine Parameter übergeben werden, oder den Zeitstempel der DateTime-Zeichenfolge an, wenn ihr übergeben wird. Unterstützte DateTime-Formate sind W3C-DTF und RFC 1123.
| Val (V DoubleVec, doppelte i)        | Doppelte        | Gibt den Wert des Elements, das am Speicherort i im Vektor V, mit einem Anfangs Index 0 (null) ist.

Einige der Funktionen, die in der Tabelle oben beschriebenen können eine Liste als Argument übernehmen. Die durch Trennzeichen getrennte Liste ist eine beliebige Kombination von *doppelten* und *DoubleVec*. Beispiel:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Der Wert *DoubleVecList* wird in einer einzelnen *DoubleVec* vor der Auswertung konvertiert. Angenommen, wenn `v = [1,2,3]`, dann aufrufen `avg(v)` entspricht dem Aufrufen von `avg(1,2,3)`. Aufrufen von `avg(v, 7)` entspricht dem Aufrufen von `avg(1,2,3,7)`.

## <a name="a-namegetsampledataaobtain-sample-data"></a><a name="getsampledata"></a>Abrufen von Beispieldaten

Formeln automatisch skalieren wirken sich auf Kennzahlen Daten (Beispiele), die durch den Stapel-Dienst bereitgestellt wird. Eine Formel vergrößert oder verkleinert Ressourcenpool Größe basierend auf den Werten, die sie aus dem Dienst abruft. Die oben beschriebenen Variablen Dienst definiert sind, bereitstellen, verschiedene Methoden zum Zugriff auf Daten, die das Objekt zugeordnet ist. Der folgende Ausdruck zeigt beispielsweise eine Anforderung an die letzten fünf Minuten der CPU-Auslastung zu erhalten:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Beschreibung |
| --- | --- |
| GetSample() | Die `GetSample()` Methode gibt einen Vektor von Daten (Beispiele).<br/><br/>Ein Beispiel ist 30 Sekunden im Wert von Kennzahlen Daten. Kurzum, werden Beispiele alle 30 Sekunden abgerufen. Wie unten angegeben, es gibt jedoch eine Verzögerung zwischen wann eine Stichprobe gesammelt werden, und wenn sie eine Formel zur Verfügung steht. Als solche möglicherweise nicht alle Beispiele für einen bestimmten Zeitraum zur Auswertung von einer Formel zur Verfügung.<ul><li>`doubleVec GetSample(double count)`<br/>Gibt die Anzahl von Beispielen, um die letzte Beispiele zu ermitteln, die gesammelt wurden.<br/><br/>`GetSample(1)`Gibt das letzte verfügbare Beispiel. Für Kennzahlen wie `$CPUPercent`, jedoch Dies sollte nicht verwendet werden, da es ist nicht möglich, *Wenn* Informationen im Beispiel gesammelt wurde. Es möglicherweise zuletzt verwendete, oder aufgrund System, ist es möglicherweise viel ältere. Es empfiehlt sich in diesem Fall verwenden Sie ein Zeitintervall wie unten dargestellt.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Gibt einen Zeitrahmen für das Erfassen von Beispieldaten an. Optional, gibt sie den Prozentsatz der Beispiele, in denen die angeforderten Zeitrahmens verfügbar sein müssen.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`20 Beispiele würde zurückgegeben werden, wenn alle Beispiele für die letzten 10 Minuten im Verlauf CPUPercent vorhanden sind. Wenn der letzten Minute Unterhaltungsverlauf nicht verfügbar war, würde jedoch nur 18 Beispiele zurückgegeben. In diesem Fall:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`fehl, weil nur 90 % der Beispiele zur Verfügung stehen.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`wäre erfolgreich.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Gibt einen Zeitrahmen für das Erfassen von Daten mit einer Startzeit und eine Endzeit an.<br/><br/>Wie zuvor erwähnt, ist es eine Verzögerung zwischen wann eine Stichprobe gesammelt werden, und wenn sie eine Formel zur Verfügung steht. Dies betrachtet, die bei Verwendung der `GetSample` Methode. Finden Sie unter `GetSamplePercent` unter.|
| GetSamplePeriod() | Gibt den Zeitraum Beispiele, in denen geöffnet wurden in einer Datenmenge zurückliegenden Stichprobe. |
| Count() | Gibt die Gesamtzahl der Beispiele im metrischen protokolliert. |
| HistoryBeginTime() | Gibt den Zeitstempel der Stichprobe für die Metrik ältesten Daten zur Verfügung. |
| GetSamplePercent() |Gibt den Prozentsatz der verfügbaren Beispiele für ein angegebenes Zeitintervall an. Beispiel:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Da der `GetSample` Methode schlägt fehl, wenn Sie der Prozentsatz der Beispiele zurückgegeben wird kleiner als der `samplePercent` angegeben haben, können Sie die `GetSamplePercent` Methode, um zuerst zu überprüfen. Dann können Sie eine alternative Aktion ausführen, wenn Sie nicht genügend Beispiele vorhanden sind, ohne die automatische Skalierung Auswertung anhalten.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Beispiele, Prozentsatz für die Stichprobe und die *GetSample()* -Methode

Der Vorgang Core neben einer Formel automatisch skalieren ist Vorgangs- und Ressourcenfelder metrische Daten abrufen, und passen Sie dann auf Ressourcenpool Größe basierend auf diesen Daten. Daher ist es wichtig, dass eine klare Vorstellung davon Interaktion automatisch skalieren Formeln mit Kennzahlen Daten oder "Beispiele".

**Beispiele**

Stapel Dienst regelmäßig *Beispiele* des Vorgangs- und Ressourcenfelder Kennzahlen akzeptiert und verfügbar sind in Formeln automatisch skalieren. In diesen Beispielen werden alle 30 Sekunden vom Dienst Stapel aufgezeichnet. Es ist jedoch in der Regel einige Wartezeit, bei dem eine Verzögerung zwischen Wenn diese Beispiele aufgezeichnet wurden, und wenn sie zur Verfügung gestellt werden (und von gelesen werden können) automatisch skalieren Formeln. Darüber hinaus aufgrund von verschiedenen Faktoren wie Netzwerk- oder andere Infrastrukturprobleme, Beispiele möglicherweise nicht für ein bestimmtes Intervall aufgezeichnet wurde. Das Ergebnis "fehlenden" Beispiele.

**Prozentsatz für die Stichprobe**

Wenn `samplePercent` zu übergeben ist die `GetSample()` Methode oder die `GetSamplePercent()` wird aufgerufen, "%" bezieht sich auf die gesamte *mögliche* Anzahl von Beispielen, die vom Dienst Stapelverarbeitung eingetragen sind und die Anzahl der tatsächlich Beispiele *zur Verfügung* zu Ihrer Formel automatisch skalieren verglichen.

Sehen wir uns Zeitspanne 10 Minuten als Beispiel. Da Beispiele alle 30 Sekunden, innerhalb von 10 Minuten Zeitspanne, aufgezeichnet werden wäre die maximale Gesamtzahl der Beispiele, in denen von Stapelverarbeitung eingetragen sind 20 Beispiele (2 pro Minute). Jedoch aufgrund der gehörende Wartezeit des Berichterstellungsmechanismus oder ein anderes Problem innerhalb der Azure-Infrastruktur, es möglicherweise nur 15 Beispiele, in denen die Formel automatisch skalieren zum Lesen zur Verfügung stehen. Dies bedeutet, dass für den betreffenden Zeitraum 10 Minuten nur **75 %** der Gesamtzahl der Beispiele aufgezeichnet die Formel tatsächlich verfügbar sind.

**Beispiel für und GetSample() Bereiche**

Automatisch skalieren Formeln werden jetzt vergrößern und Verkleinern die Pools – Knoten hinzufügen oder Entfernen von Knoten sein. Da Knoten Geld Kosten, möchten Sie sicherstellen, dass Ihre Formeln eine intelligente Analyse-Methode verwenden, die auf ausreichend Daten basiert. Wir empfehlen daher, dass Sie eine Analyse Trends Typ in Formeln verwendet werden. Dieses Typs wird vergrößern und Verkleinern der Pools basierend auf einen *Bereich* von erfassten Stichproben.

Verwenden Sie hierzu `GetSample(interval look-back start, interval look-back end)` Beispiele einen **Vektor** zurück:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Wenn die oben angegebenen Linie durch den Stapel ausgewertet wird, wird einen Bereich der Beispiele als Vektor von Werten zurückgegeben. Beispiel:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Nachdem Sie die Vektorversion von Beispielen gesammelt haben, können Sie dann Funktionen wie `min()`, `max()`, und `avg()` aussagekräftige Werte aus dem Bereich zusammengestellten abgeleitet werden.

Zur Erhöhung der Sicherheit können Sie eine Formel Auswertung *fehlschlägt* erzwingen, wenn kleiner als Prozentwert bestimmte Beispiel für einen bestimmten Zeitraum verfügbar ist. Wenn Sie eine Formel Auswertung fehlschlägt erzwingen, weisen Sie Stapel einzustellen weiteren Auswertung der Formel aus, wenn der angegebene Prozentsatz des Beispiele nicht verfügbar – ist und wird keine Änderung auf Ressourcenpool Größe vorgenommen werden. Prozentwert erforderlichen Beispiele für die Bewertung erfolgreich ist, geben sie als dritten Parameter zu `GetSample()`. Hier ist eine Vorbedingung 75 % von Beispielen angegeben:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Es ist es wichtig, aufgrund der oben erwähnten Verzögerung bei der Stichprobe Verfügbarkeit, immer eines Zeitraums mit einer aussehen-Back Startzeit angeben, die älter als einer Minute ist. Dies ist, da es ungefähr eine Minute für Beispiele, wenn das System verbreitet dauert also Beispiele im Bereich `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` häufig nicht zur Verfügung. Erneut, können Sie den Prozentsatz-Parameter der `GetSample()` zu erzwingen, dass die Anforderung einer bestimmten Beispiels Prozentsatz.

> [AZURE.IMPORTANT] Wir **wird dringend empfohlen** , Sie * *verlassen Sie sich *nur* auf `GetSample(1)` in Ihrer Formeln automatisch skalieren **. Dies ist, da `GetSample(1)` im Wesentlichen besagt an den Dienst Stapel "brauche im letzte Beispiel verfügen, ganz gleich, wie vor kurzem Sie verfügen." Da es nur eine einzelne Probe ist und möglicherweise eine ältere Stichproben, kann es nicht Supportmitarbeiter des letzten Vorgang oder eine Ressource Zustand des Bilds größer sein. Wenn Sie verwenden, gehen Sie wie folgt `GetSample(1)`, stellen Sie sicher, dass es als Teil einer größeren-Anweisung und nicht die einzige Datenpunkt, der die Formel benötigt wird.

## <a name="metrics"></a>Kennzahlen

Wenn Sie eine Formel definieren sind, können Sie sowohl **Ressourcen-** und **Aufgabendaten** Kennzahlen verwenden. Sie passen Sie die Ziel-Anzahl der dedizierten Knoten im Pool basierend auf den Kennzahlen Daten, die Sie erhalten und ausgewertet werden soll. Finden Sie im Abschnitt [Variablen](#variables) über weitere Informationen auf jede Metrik ein.

<table>
  <tr>
    <th>Metrisch</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><b>Ressource</b></td>
    <td><p><b>Ressource Kennzahlen</b> basieren auf die CPU-Bandbreite und Arbeitsspeicher Verwendung von Datenverarbeitungsknoten als auch die Anzahl der Knoten.</p>
        <p> Diese Variablen Dienst definiert sind nützlich für basierend auf Knoten zählen Anpassungen vornehmen:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Diese Variablen Dienst definiert sind nützlich für basierend auf Knoten Ressource: Einsatz Anpassungen vornehmen:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Aufgabe</b></td>
    <td><p><b>GUID des Kennzahlen</b> basierend auf den Status von Vorgängen, wie z. B. aktiv, Ausstehend, und ausgefüllt werden. Die folgenden Variablen Dienst definiert sind nützlich für Pool-Größe Anpassungen Grundlage Vorgang Metrik vornehmen:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Schreiben einer Formel automatisch skalieren

Sie erstellen eine Formel automatisch skalieren bilden Anweisungen, die oben aufgeführten Komponenten verwenden und dann diese Anweisungen in eine vollständige Formel kombinieren. In diesem Abschnitt erstellen wir eine automatisch skalieren Beispielformel, die einige praktisches Skalierung Entscheidungen ausführen können.

Zunächst definieren wir die Anforderungen für unsere neue automatisch skalieren Formel. Die Formel sollte wie folgt:

1. **Erhöhen** der Ziel-Anzahl berechnen-Knoten in einem Ressourcenpool ist CPU-Auslastung hoch.
2. **Verkleinern** der Ziel-Anzahl berechnen-Knoten in einem Ressourcenpool, wenn CPU-Auslastung niedrig ist.
3. Beschränken Sie die **maximale** Anzahl von Knoten immer bis 400.

*Erhöhen Sie* die Anzahl der Knoten während hoher CPU-Auslastung definieren wir die Anweisung, die eine benutzerdefinierte Variable auffüllt (`$totalNodes`) mit einem Wert, der 110 % der aktuellen Ziel Anzahl von Knoten, jedoch nur, wenn die minimale durchschnittliche CPU-Auslastung während der letzten 10 Minuten wurde, über 70 %. Andernfalls verwenden Sie den aktuellen dedizierten Wert.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

*Verringern* Sie die Anzahl der Knoten während der CPU-Auslastung niedrig, legt die nächste Anweisung in unseren Formel derselben `$totalNodes` Variable auf 90 % der aktuellen Ziel Anzahl Knoten, wenn die durchschnittliche CPU-Auslastung in den letzten 60 Minuten unter 20 % wurde. Verwenden Sie andernfalls den aktuellen Wert der `$totalNodes` , die wir in der obigen Anweisung ausgefüllt.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Beschränken Sie Ziel Anzahl von dedizierten Computeknoten auf eine **maximale** von 400 jetzt:

```
$TargetDedicated = min(400, $totalNodes)
```

Dies ist die vollständige Formel ein:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Erstellen einer Ressourcenpool automatisch skalieren aktiviert

Um einen neuen Pool mit Automatische Skalierung aktiviert zu erstellen, können Sie eine der folgenden Verfahren verwenden:

**Stapel .NET**

1. Erstellen Sie den Pool mit [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. Legen Sie die Eigenschaft [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) auf `true`.
1. Legen Sie die Eigenschaft [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) mit der Formel automatisch skalieren.
1. (Optional) Legen Sie die Eigenschaft [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (Standard ist 15 Minuten).
1. Übernehmen Sie den Pool mit [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) oder [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**Stapel REST-API**

* [Hinzufügen einer Ressourcenpool mit einer Firma](https://msdn.microsoft.com/library/azure/dn820174.aspx): Geben Sie die `enableAutoScale` und `autoScaleFormula` Elemente in der REST-API so konfigurieren Sie die automatische Skalierung für einen Pool, wenn Sie sie erstellt haben.

Der folgende Codeausschnitt erstellt ein Ressourcenpool automatisch skalieren aktiviert mithilfe der [Stapel .NET] [ net_api] Bibliothek. Der Pool automatisch skalieren Formel legt die Ziel-Anzahl der Knoten auf 5 montags und 1 auf jeden zweiten Tag der Woche. Das [Intervall für die automatische Skalierung](#automatic-scaling-interval) wird auf 30 Minuten festgelegt. In dieser und den anderen C#-Codeausschnitte in diesem Artikel, "MyBatchClient" ist eine ordnungsgemäß initialisierte Instanz der [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Zusätzlich zu den Stapel REST-API und .NET SDK können Sie eine der anderen [Stapel SDKs](batch-technical-overview.md#batch-development-apis), [Stapel PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md)und den [Stapel CLI](batch-cli-get-started.md) automatische Skalierung konzipiert verwenden.

> [AZURE.IMPORTANT] Wenn Sie ein automatisch skalieren aktiviert Ressourcenpool erstellen, müssen Sie **nicht** angeben der `targetDedicated` Parameter. Darüber hinaus nach Wunsch manuell ändern der Größe einer Ressourcenpool automatisch skalieren aktiviert (z. B. mit [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), dann müssen Sie ersten **Deaktivieren der** automatischen Skalierung nicht auf dem Pool, Ändern der Größe.

### <a name="automatic-scaling-interval"></a>Intervall für die automatische Skalierung

Standardmäßig passt der Stapel Dienst ein Ressourcenpool Größe anhand der Formel automatisch skalieren alle **15 Minuten**. Dieses Intervall ist konfigurierbare, jedoch mithilfe der folgenden Ressourcenpool Eigenschaften:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Stapel .NET)
* [AutoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST-API)

Das kleinste Intervall beträgt fünf Minuten, und das Maximum beträgt 168 Stunden. Wenn ein Intervall außerhalb dieses Bereichs angegeben wird, wird der Stapel-Dienst einen Fehler Ungültige Anforderung (400) zurückgeben.

> [AZURE.NOTE] Automatische Skalierung ist derzeit nicht reagieren auf Änderungen in weniger als einer Minute vorgesehen, aber lieber zum Anpassen der Größe Ihrer Ressourcenpool allmählich während der Ausführung einer Arbeitsbelastung vorgesehen ist.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Aktivieren Sie automatische Skalierung auf einem vorhandenen pool

Wenn Sie mithilfe des Parameters *TargetDedicated* bereits einen Pool mit einer festgelegten Anzahl von Computeknoten erstellt haben, können Sie weiterhin automatische Skalierung im Pool aktivieren. Jeder Stapel SDK bietet eine Operation "aktivieren automatisch skalieren", beispielsweise:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (Stapel .NET)
*  [Aktivieren der automatischen Skalierung auf einem Ressourcenpool] [ rest_enableautoscale] (REST-API)

Wenn Sie die automatische Skalierung auf einem vorhandenen Pool aktivieren, gilt Folgendes:

* Wenn automatische Skalierung aktuell ist im Pool **deaktiviert** , wenn Sie die Anfrage "aktivieren automatisch skalieren" registrieren, angeben *müssen* Sie eine gültige automatisch skalieren Formel, wenn Sie die Anfrage registrieren. *Optional* können Sie ein automatisch skalieren Auswertung Intervall festlegen. Wenn Sie ein Intervall nicht angeben, ist der Standardwert von 15 Minuten verwendet.

* Wenn automatisch skalieren aktuell auf dem Pool **aktiviert** ist, können Sie eine Formel automatisch skalieren, ein Intervall Auswertung oder beides angeben. Sie können nicht beide Eigenschaften auslassen.

  * Wenn Sie ein neues automatisch skalieren Auswertung Intervall angeben möchten, klicken Sie dann der vorhandenen Auswertung Zeitplan wird beendet, und einem neuen Projektplan wird gestartet. Beginnt der neue Zeitplan ist der Uhrzeit, an der die Anfrage "automatisch skalieren aktivieren" ausgestellt wurde.

  * Wenn Sie die Formel automatisch skalieren oder Auswertung Intervall, den Stapel Dienst weglassen weiterhin den aktuellen Wert der diese Einstellung verwenden.

> [AZURE.NOTE] Wenn ein Wert für den Parameter *TargetDedicated* angegeben wurde, bei der Pool erstellt wurde, wird es ignoriert, wenn die automatische Skalierung Formel ausgewertet wird.

Dieser C#-Codeausschnitt verwendet die [Stapel .NET] [ net_api] -Bibliothek auf automatische Skalierung auf einem vorhandenen Pool zu aktivieren:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Aktualisieren einer Formel automatisch skalieren

Sie verwenden derselben "automatisch skalieren aktivieren" Anforderung *Aktualisieren* Sie die Formel auf einem vorhandenen automatisch skalieren aktiviert Pool (z. B. mit [EnableAutoScale] [ net_enableautoscale] in .NET Stapel). Es gibt keine spezielle "aktualisieren automatisch skalieren" Operation aus. Angenommen, wenn automatische Skalierung bereits auf "Myexistingpool" aktiviert ist, wenn Sie der folgende Code ausgeführt wird, deren Formel automatisch skalieren wird ersetzt mit dem Inhalt des `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Aktualisieren Sie das Intervall automatisch skalieren

Wie bei der Aktualisierung einer Formel automatisch skalieren den gleichen [EnableAutoScale] [ net_enableautoscale] Methode, um das Zeitintervall automatisch skalieren Auswertung eines vorhandenen automatisch skalieren aktiviert Ressourcenpool zu ändern. Wenn Sie beispielsweise das automatisch skalieren Auswertung Intervall auf 60 Minuten für einen Pool festlegen, die bereits automatisch skalieren aktiviert ist:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Auswerten einer Formel automatisch skalieren

Sie können eine Formel auswerten, bevor Sie sie zu einem Ressourcenpool anwenden. Auf diese Weise können Sie ausführen, eine "Testlauf" der Formel auswerten von seinem Abschluss Punkte, bevor Sie die Formel in Betrieb genommen angezeigt.

Um eine Formel automatisch skalieren ausgewertet werden soll, müssen Sie zuerst die **Automatische Skalierung aktivieren** auf dem Pool mit einer **gültigen Formel**. Wenn Sie eine Formel auf einem Ressourcenpool zu testen, die automatische Skalierung aktiviert noch nicht möchten, können Sie die Formel einzeiligen `$TargetDedicated = 0` Wenn Sie automatische Skalierung zuerst aktivieren. Verwenden Sie dann eine der folgenden für die Formel ausgewertet werden soll, die Sie testen möchten:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) oder [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Diese Methoden Stapel .NET erfordern die ID von einer vorhandenen Ressourcenpool und eine Zeichenfolge, die mit der Formel automatisch skalieren für ausgewertet werden soll. Ergebnisse der Bewertung sind in der zurückgegebenen [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) Instanz enthalten.

* [Eine automatische Skalierung Formel auswerten](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Geben Sie diese Anforderung REST-API die Pool-ID in der URI und die Formel automatisch skalieren im *AutoScaleFormula* -Element des Anforderungstexts. Die Antwort des Vorgangs enthält, die mit der Formel verknüpft werden möglicherweise Fehlerinformationen an.

In diesem [Stapel .NET] [ net_api] Codeausschnitt, wir eine Formel, indem sie auf die [CloudPool]anwenden auswerten[net_cloudpool]. Wenn der Ressourcenpool keine automatische Skalierung aktiviert hat, aktivieren Sie wir es zuerst.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Erfolgreichen Auswertung der Formel in dieser Ausschnitt führt zu Fehlern in der Ausgabe ähnlich wie die folgende:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Erhalten von Informationen zu skalieren wird ausgeführt

Um sicherzustellen, dass die Formel ausführt, wie erwartet, empfehlen wir, dass Sie regelmäßig prüfen der Ergebnisse der automatische Skalierung "ausgeführt" Stapel für Ihre Ressourcenpool ausführt. Führen Sie also (oder aktualisieren) einen Verweis auf den Pool, und überprüfen Sie die Eigenschaften für die letzte automatisch skalieren ausführen.

In .NET Stapel weist die Eigenschaft [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) verschiedene Eigenschaften bereitstellen von Informationen über die neuesten automatische Skalierung ausgeführte im Pool durch den Stapel Dienst ausführen.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

In die REST-API gibt die [Informationen zu einem Ressourcenpool Get](https://msdn.microsoft.com/library/dn820165.aspx) -Anforderung Informationen zu dem Pool, wozu auch die neuesten automatische Skalierung Informationen in [AutoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)ausführen.

Der folgende C#-Codeausschnitt verwendet die Stapel .NET-Bibliothek zum Drucken von Informationen zu den letzten automatische Skalierung Ressourcenpool "MyPool" ausgeführt:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Beispielausgabe des vorhergehenden Codeausschnitt:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Automatisch skalieren Beispielformeln

Werfen Sie einen Blick auf einige Formeln, die verschiedene Methoden zum Anpassen des berechnen von Ressourcen in einem Ressourcenpool anzeigen aus.

### <a name="example-1-time-based-adjustment"></a>Beispiel 1: Zeitbasierte Anpassung

Vielleicht möchten Sie die Poolgröße basierend auf den Tag der Woche und die Uhrzeit des Tages, zum Vergrößern oder verkleinern die Anzahl der Knoten im Pool entsprechend anpassen.

Diese Formel erhält zuerst die aktuelle Uhrzeit aus. Ist eine Weekday (1-5) und innerhalb der Arbeitszeit (8 Uhr), wird die Größe des Target Ressourcenpool auf 20 Knoten festgelegt. Andernfalls wird es auf 10 Knoten festgelegt.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Beispiel 2: Aufgabenbasierten Anpassung

In diesem Beispiel wird die Größe der Ressourcenpool basierend auf die Anzahl der Aufgaben in der Warteschlange angepasst. Beachten Sie, dass sowohl Kommentare als auch Zeilenumbrüche in einer Formel Zeichenfolgen verwendet werden.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Beispiel 3: Buchhaltung für parallele Aufgaben

Dies ist ein weiteres Beispiel, das die Grundlage der Anzahl von Vorgängen Poolgröße angepasst werden. Mit dieser Formel werden berücksichtigt auch die [MaxTasksPerComputeNode] [ net_maxtasks] Wert, der für den Pool festgelegt wurde. Dies ist besonders hilfreich in Situationen, die Stelle, an der [Ausführung der Aufgabe parallel](batch-parallel-node-tasks.md) auf Ihre Ressourcenpool aktiviert wurde.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Beispiel 4: Einrichten einer anfänglichen Pool-Größe

Dieses Beispiel zeigt einen C#-Codeausschnitt mit einer Formel automatisch skalieren, die die Größe der Ressourcenpool auf eine bestimmte Anzahl von Knoten für einen anfänglichen Zeitraum festlegt. Und sie die Poolgröße basierend auf der Anzahl der Ausführung und aktiven angepasst werden Vorgänge nach die anfänglichen Zeitspanne abgelaufen ist.

Die Formel in den folgenden Codeausschnitt:

- Legt die Größe der anfänglichen Pool vier Knoten.
- Die Größe der Ressourcenpool wird nicht innerhalb der ersten 10 Minuten dem Pool des Lebenszyklus angepasst werden.
- Nach 10 Minuten erhält der Maximalwert die Anzahl der laufenden und aktive Aufgaben innerhalb der letzten 60 Minuten.
  - Wenn beide Werte sind 0 (d. h., dass keine Aufgaben laufenden oder in den letzten 60 Minuten aktiviert wurden), wird die Größe der Ressourcenpool auf 0 festgelegt.
  - Wenn Sie entweder Wert größer als 0 (null) ist, wird keine Änderung vorgenommen.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Nächste Schritte

* [Maximieren Azure Stapel berechnen Ressource: Einsatz bei gleichzeitiger Knoten Aufgaben](batch-parallel-node-tasks.md) enthält Details, wie Sie mehrere Aufgaben auf den Knoten berechnen im Pool gleichzeitig ausführen können. Über das automatische Skalierung kann dieses Feature unterstützen, zum Verringern der Dauer des Projekts für einige Auslastung, indem Sie Kosten einsparen.

* Ein anderes Effizienz Booster sicher, dass eine Anwendung Stapel Stapel Dienst in die am besten geeignete Methode in Abfragen. In der [Abfrage auf des Diensts Azure Stapel effiziente](batch-efficient-list-queries.md)erfahren Sie, wie Sie die Menge der Daten zu beschränken, die der Übertragung schneidet, wenn Sie den Status der potenziell Tausende von Datenverarbeitungsknoten oder Aufgaben Abfragen.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
