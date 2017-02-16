<properties
 pageTitle="So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler"
 description="So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="how-to-build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler  

## <a name="overview"></a>(Übersicht)

Das Herzstück von einer Azure Scheduler ist Aufgabe den *Terminplan*. Zeitplan bestimmt, wann und wie der Planer den Auftrag ausführt.

Azure Scheduler können Sie verschiedene einmalige und periodische Terminpläne für ein Projekt angeben. *Einmalige* Terminpläne effektiv einmal zu einer bestimmten Zeit – auslösen, sind sie *wiederkehrende* Zeitpläne, die nur einmal ausgeführt. Wiederkehrende Zeitpläne Fire auf einer vordefinierten Häufigkeit.

Mit dieser Flexibilität bietet folgende Vorteile Azure Scheduler eine Vielzahl von Business-Szenarien unterstützen:

-   Periodische Daten Aufräumen – z. B. täglich, löschen Sie alle Tweets, die älter als 3 Monate
-   Archivierung – z. B. jeden Monat Rechnung Verlauf Sicherung Dienst Pushbenachrichtigungen
-   Anforderung von externen Daten – herausziehen z. B. alle 15 Minuten, neue Ski Wetterbericht aus NOAA
-   Abbildung des Verarbeitung – z. B. jeder Wochentag in Zeiten, verwenden Sie Cloud computing, um Bilder komprimieren an diesem Tag hochgeladen


In diesem Artikel durchgehen wir Beispiel Einzelvorgänge, die Sie mit Azure Scheduler erstellen können. Wir erläutern die JSON-Daten, die jeden Zeitplan zu beschreiben. Wenn Sie die [REST-API Scheduler](https://msdn.microsoft.com/library/mt629143.aspx)verwenden, können Sie diese derselben JSON zum [Erstellen eines Auftrags Azure Scheduler](https://msdn.microsoft.com/library/mt629145.aspx)verwenden.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Die vielen Beispielen in diesem Thema veranschaulichen die große Bandbreite von Szenarien, die Azure Scheduler unterstützt. In diesen Beispielen verdeutlichen gestreut, zum Erstellen von Zeitplänen für viele Verhaltensmustern, einschließlich der folgenden:

-   Führen Sie einmal an einem bestimmten Datum und Uhrzeit
-   Ausführen und explizite mehrmals wiederholenden
-   Sofort ausführen und als Serie
-   Führen Sie aus und wiederholenden Sie alle *n* Minuten, Stunden, Tagen, Wochen oder Monaten, beginnend ab einem bestimmten Zeitpunkt
-   Führen Sie aus und wiederholenden Sie am wöchentliche oder monatliche Häufigkeit aber nur an bestimmten Tagen, bestimmte Tage der Woche oder bestimmte Tage des Monats
-   Führen Sie aus und wiederholenden Sie am mehrmals in einem Zeitraum von – z. B. letzten Freitag und Montag oder bei 5:15 Uhr und 5:15 pm jeden Tag des Monats

## <a name="dates-and-datetimes"></a>Datums- und Zeiteingabe

Datumsangaben in Azure Scheduler Aufträge führen Sie die [ISO-8601 Spezifikation](http://en.wikipedia.org/wiki/ISO_8601) und Einbeziehung von Datum.

Datum-/ Uhrzeit-verweisen in Azure Scheduler Aufträge folgen der [ISO-8601 Spezifikation](http://en.wikipedia.org/wiki/ISO_8601) und Datum und die Uhrzeit Teile einschließen. Ein Datum / Uhrzeit, der nicht UTC-Offset wird als UTC werden angenommen.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Gewusst wie: Verwenden von JSON und REST-API zum Erstellen von Zeitplänen

So erstellen einen einfachen Zeitplan mithilfe der [Azure Scheduler REST-API](https://msdn.microsoft.com/library/mt629143), erste [Registrieren Sie Ihr Abonnement mit einer Ressourcenanbieter](https://msdn.microsoft.com/library/azure/dn790548.aspx) (der Name des Anbieters für Scheduler ist _Microsoft.Scheduler_), klicken Sie dann auf [Erstellen einer Websitesammlung Position](https://msdn.microsoft.com/library/mt629159.aspx), und schließlich [ein Projekt erstellen](https://msdn.microsoft.com/library/mt629145.aspx). Wenn Sie ein Projekt erstellen, können Sie die Planung und die Verwendung von JSON wie das entnommen unter Serie angeben:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often to fire (default to 1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default to recur infinitely)
            "endTime": "2012-11-04",      // optional (default to recur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Übersicht: Auftrag Schema Grundlagen

Die folgende Tabelle enthält einen detaillierten Überblick über die wichtigsten Elemente im Zusammenhang mit Serie, und planen ein Projekt:

|**JSON-name**|**Beschreibung**|
|:--|:--|
|**_Startzeit_**|_Startzeit_ ist ein Datum-/ Uhrzeit. Einfache Terminpläne _Startzeit_ entspricht dem ersten Vorkommen und der Auftrag beginnt für komplexe Terminpläne, frühestens _Startzeit_.|
|**_Serie_**|Gibt an, das Objekt _Serie_ Serie Regeln für das Projekt, und die Position die Serie werden mit ausgeführt. Das Objekt Serie unterstützt die Elemente _Häufigkeit, Intervall, Endzeit, Anzahl,_ und _Planen_. Wenn _Serie_ definiert ist, ist _Häufigkeit_ erforderlich. die anderen Elemente der _Serie_ sind optional.|
|**_Häufigkeit_**|Die _Häufigkeit_ Zeichenfolge zur Darstellung der Häufigkeit Maßeinheit, der Auftrag erneut auftritt. Unterstützte Werte sind _"Minute", "Stunde", "Day", "Woche"_ oder _"Monat"._|
|**_Intervall_**|Das _Intervall_ eine positive ganze Zahl ist, und kennzeichnet das Intervall für die _Häufigkeit_ , die bestimmt, wie oft der Auftrag ausgeführt wird. Wenn _Intervall_ 3 und _die Häufigkeit_ ist "Woche", wiederholt der Auftrag 3 Wochen. Azure Scheduler unterstützt eine maximale _Intervall_ 18 Monate für monatliche Häufigkeit 78 Wochen für Wöchentliche Häufigkeit oder 548 Tagen für tägliche Häufigkeit. Stunde und Minute Häufigkeit, ist der unterstützten Bereichs 1 < = _Intervall_ < = 1000.|
|**_Endzeit_**|Die Zeichenfolge _Endzeit_ gibt der Datum-/ Uhrzeit Vergangenheit dem der Auftrag nicht ausgeführt wird. Es ist nicht zulässig, eine _Endzeit_ in der Vergangenheit. Wenn keine _Endzeit_ oder Count angegeben ist, wird die Position endlos ausgeführt. Sowohl die _Endzeit_ als auch die _Count_ können nicht dasselbe Projekt einbezogen werden.|
|**_zählen_**|<p>Die _Anzahl_ ist eine positive ganze Zahl (größer als 0 (null)), die angibt, wie oft, die vor dem Durchführen dieses Auftrag ausgeführt werden soll.</p><p>Die _Anzahl_ stellt die Anzahl der Häufigkeit, mit die der Job ausgeführt wird, bevor bestimmt wird als abgeschlossen. Für ein Projekt, das mit _zählen_ 5 und Anfangstermin für Montag täglich ausgeführt wird, führt der Auftrag nach der Ausführung am Freitag. Wenn das Startdatum in der Vergangenheit liegt, wird die erste Ausführung von der Erstellungszeit berechnet.</p><p>Wenn keine _Endzeit_ oder _Count_ angegeben ist, wird die Position endlos ausgeführt. Sowohl die _Endzeit_ als auch die _Count_ können nicht dasselbe Projekt einbezogen werden.</p>|
|**_Zeitplan_**|Ein Projekt mit einer bestimmten Häufigkeit ändert deren Grundlage eines Zeitplans Serie Serie. Einen _Zeitplan_ enthält basierte auf Stunden, Minuten, Wochentage, Monat Tage und Wochennummer Änderungen an.|

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Übersicht: Auftrag Schema Standardeinstellungen, Grenzwerte und Beispiele

Nach dieser Übersicht betrachten wir jedes dieser Elemente im Detail.

|**JSON-name**|**Value-Typ**|**Erforderlich?**|**Standardwert**|**Gültige Werte**|**Beispiel**|
|:---|:---|:---|:---|:---|:---|
|**_Startzeit_**|Zeichenfolge|Nein|Keine|ISO-8601 Datum / Uhrzeit|<code>"startTime" : "2013-01-09T09:30:00-08:00"</code>|
|**_Serie_**|Objekt|Nein|Keine|Serie Objekt|<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code>|
|**_Häufigkeit_**|Zeichenfolge|Ja|Keine|"Minute", "Stunde", "Day", "Woche", "Monat"|<code>"frequency" : "hour"</code> |
|**_Intervall_**|Zahl|Nein|1|1 und 1000.|<code>"interval":10</code>|
|**_Endzeit_**|Zeichenfolge|Nein|Keine|Darstellung einer Uhrzeit in der Zukunft Datum-/ Uhrzeit-Wert|<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
|**_zählen_**|Zahl|Nein|Keine|> = 1|<code>"count": 5</code>|
|**_Zeitplan_**|Objekt|Nein|Keine|Zeitplan-Objekt|<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code>|

## <a name="deep-dive-starttime"></a>Aneignen: _Startzeit_

In der folgenden Tabelle erfasst werden, wie die _Startzeit_ Steuerelemente wie ein Projekt ausgeführt wird.

|**Startzeitwert**|**Keine Serie**|**Serie. Kein Zeitplan**|**Serie mit Zeitplan**|
|:--|:--|:--|:--|
|**Keine Startzeit**|Sofort einmal ausführen|Einmal sofort ausführen. Führen Sie die nachfolgende Ausführungen basierend auf die Berechnung der Zeitpunkt der letzten Ausführung|<p>Sofort einmal ausführen</p><p>Führen Sie die nachfolgende Ausführungen basierend auf Serientyp Zeitplan</p>|
|**Startzeit in der Vergangenheit**|Sofort einmal ausführen|<p>Berechnen von zukünftigen Ausführung erstmals nach der Anfangszeit, und führen Sie zu diesem Zeitpunkt</p><p>Zeitpunkt der letzten Ausführung nachfolgende Ausführungen basierend Oncalculating ausführen</p><p>Siehe Beispiel nach dieser Tabelle mehr zum Thema</p>|<p>Auftrag startet _nicht sooner als_ die angegebene Startzeit. Das erste Vorkommen basiert auf den Terminplan aus der Startzeit berechnet</p><p>Führen Sie die nachfolgende Ausführungen basierend auf Serientyp Zeitplan</p>|
|**Startzeit in Zukunft oder am präsentieren**|Führen Sie einmal zur angegebenen Startzeit|<p>Führen Sie einmal zur angegebenen Startzeit</p><p>Führen Sie die nachfolgende Ausführungen basierend auf die Berechnung der Zeitpunkt der letzten Ausführung</p>|<p>Auftrag startet _nicht sooner als_ die angegebene Startzeit. Ersten Auftretens basiert auf den Terminplan aus der Startzeit berechnet</p><p>Führen Sie die nachfolgende Ausführungen basierend auf Serientyp Zeitplan</p>|

Sehen Sie ein Beispiel für was passiert, wobei _Startzeit_ in der Vergangenheit mit _Serie_ , aber kein _Zeitplan_ist ein.  Davon aus, dass die aktuelle Uhrzeit 2015-04-08 13:00, _Startzeit_ ist 2015-04-07 14:00 Uhr und _Serie_ ist alle 2 Tage (mit _Häufigkeit_definiert: Tag und _Intervall_: 2.) Notiz, die in der Vergangenheit ist der _Startzeit_ und tritt auf, bevor die aktuelle Uhrzeit

Unter diesen Umständen wird die _erste Ausführung_ 2015-04-09 14:00 Uhr werden\. der Scheduler-Engine berechnet Vorkommen der Ausführung von der Enduhrzeit.  Alle Instanzen in der Vergangenheit verworfen werden. Das Modul verwendet das nächste Vorkommen, das in der Zukunft liegt.  Also ist in diesem Fall _Startzeit_ 2015-04-07 2:00 Uhr, also das nächste Vorkommen 2 Tagen ab dieses Zeitraums, also 2015-04-09 2:00 Uhr.

Beachten Sie, dass die erste Ausführung würde werden die gleichen, auch wenn die Startzeit 2015-04-05 14:00 Uhr oder 2015-04-01 14:00 Uhr\. nach der ersten Ausführung werden nachfolgende Ausführungen berechnet, mit der geplanten, damit sie 2015-04-11 2:00 Uhr, dann 2015-04-13 2:00 Uhr, dann 2015-04-15 2:00 Uhr aufweisen würden usw..

Schließlich werden beim ein Projekt einen Zeitplan, hat, wenn Stunden und/oder Minuten im Zeitplan festgelegt sind, Standardwerte die Stunden und/oder Minuten der ersten Ausführung des Inhaltsverzeichnisses.

## <a name="deep-dive-schedule"></a>Tiefer: _Terminplan_

Können einerseits einen _Zeitplan_ _Grenzwert für_ die Anzahl der Position Ausführungen.  Beispielsweise wird ausgeführt Wenn ein Auftrag mit einem "Monat" Häufigkeit einen _Zeitplan_ enthält, klicken Sie auf nur Tag 31 ausführt, der Auftrag nur diese Monate, die einen Tag 31<sup>mo</sup> aufweisen.

Andererseits, können einen _Zeitplan_ auch die Anzahl der Ausführungen Position zu _Erweitern_ . Beispielsweise, wenn Sie ein Projekt mit einer Häufigkeit "Monat" einen _Zeitplan_ hat, die für den Monat Tage 1 und 2 ausgeführt wird, führt den Auftrag 1<sup>mo</sup> und 2<sup>und</sup> Tage des Monats statt nur einmal einen Monat.

Wenn mehrere Terminplan Elemente angegeben sind, wird die Reihenfolge der Auswertung von der größten zur kleinsten – Zahl Woche, Monatstag, Wochentag, Stunde und Minute.

Die folgende Tabelle beschreibt die _Terminplan_ Elemente im Detail.

|**JSON-name**|**Beschreibung**|**Gültige Werte**|
|:---|:---|:---|
|**Minuten**|Minuten der Stunde, an der der Auftrag ausgeführt wird|<ul><li>Ganze Zahl, oder</li><li>Array von ganzen Zahlen</li></ul>|
|**Stunden**|Stunden des Tages, an dem der Auftrag ausgeführt wird,|<ul><li>Ganze Zahl, oder</li><li>Array von ganzen Zahlen</li></ul>|
|**Wochentage**|Tage der Woche für das Projekt ausgeführt wird. Können nur mit einem Wöchentliche Häufigkeit angegeben werden muss.|<ul><li>"Montag", "Dienstag", "Mittwoch", "Donnerstag", "Freitag", "Samstag" oder "Sonntag"</li><li>Array der oben genannten Werte (max Arraygröße 7)</li></ul>_Nicht_ Groß-/Kleinschreibung|
|**monthlyOccurrences**|Bestimmt, welche Tage des Monats im Auftrag ausgeführt werden soll. Können nur mit einem monatlichen Häufigkeit angegeben werden muss.|<ul><li>Array von MonthlyOccurence-Objekten:</li></ul> <pre>{"Day": _Tag_<br />  "Vorkommen": _Vorkommen_<br />}</pre><p> _Tag_ wird der Tag der Woche, die der Auftrag ausgeführt werden soll, z. B. {Sonntag} jeden Sonntag des Monats ist. Erforderlich.</p><p>Vorkommen ist _Vorkommen_ des Tages während des Monats, z. B. {Sonntag,-1} ist der letzte Sonntag des Monats. Optional.</p>|
|**monthDays**|Tag des Monats, der Auftrag ausgeführt wird. Können nur mit einem monatlichen Häufigkeit angegeben werden muss.|<ul><li>Jeder Wert < =-1 und > =-31.</li><li>Jeder Wert > = 1 und < = 31.</li><li>Ein Array von über Werte</li></ul>|

## <a name="examples-recurrence-schedules"></a>Beispiele: Serie Zeitpläne

Nachfolgend werden verschiedene Beispiele für Serie Zeitpläne – Schwerpunkt auf das Objekt Zeitplan und deren untergeordnete Elemente.

Die Zeitpläne unter alle wird davon ausgegangen, dass das _Intervall_ auf 1 festgelegt ist\. eine muss außerdem angenommen werden die richtige Häufigkeit in Übereinstimmung mit im _Terminplan Neuigkeiten_ – z. B. eine kann nicht Häufigkeit "Day" verwenden und eine Änderung "MonthDays" in den Terminplan haben. Solche Einschränkungen werden oben beschrieben.

|**Beispiel**|**Beschreibung**|
|:---|:---|
|<code>{"hours":[5]}</code>|Um 5 Uhr ausführen jeden Tag. Azure Scheduler entspricht nach jeder Wert in "Stunden" mit den Wert jeder in "Minuten", einzeln zum Erstellen einer Liste von allen, die Zeiten, ist der Auftrag ausgeführt werden soll.|
|<code>{"minutes":[15], "hours":[5]}</code>|Um 5:15 Uhr ausführen jeden Tag|
|<code>{"minutes":[15], "hours":[5,17]}</code>|Führen Sie bei 5:15 Uhr und 5:15 PM jeden Tag|
|<code>{"minutes":[15,45], "hours":[5,17]}</code>|Um 5:15 Uhr, 5:45 Uhr ausführen 5:15 PM und 5:45 PM jeden Tag|
|<code>{"minutes":[0,15,30,45]}</code>|Führen Sie alle 15 Minuten|
|<code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code>|Führen Sie stündlich. Dieser Auftrag wird stündlich ausgeführt. Die Minute wird durch die _Startzeit_, gesteuert, sofern angegeben ist oder wenn keines angegeben ist, indem der Erstellungszeit. Beispielsweise ist die Start- oder der Erstellungszeit (je nachdem, was gilt) 12:25 Uhr, der Auftrag läuft am 00:25, 01:25, 02:25,..., 23:25. Zeitplan entspricht, nachdem ein Projekt mit _Häufigkeit_ von "Stunde", ein _Intervall_ von 1 und kein _Terminplan_. Der Unterschied ist, dass dieser Zeitplan mit anderen _Häufigkeit_ und _Intervall_ verwendet werden kann, andere Aufträge zu erstellen. Beispielsweise wäre die _Häufigkeit_ "Monat", würde der Zeitplan nur einmal pro Monat statt jeden Tag ausführen wäre _Häufigkeit_ "Day"|
|<code>{minutes:[0]}</code>|Führen Sie stündlich für die Stunde ein. Dieser Auftrag führt auch stündlich, aber auf die Stunde (z. B. 00 Uhr, Uhr 1, 2 Uhr usw..) Dies ist ein Projekt mit Häufigkeit von "Stunde", eine Startzeit mit 0 (null) Minuten und kein Zeitplan entspricht Wenn die Häufigkeit wurden "Day", aber die Häufigkeit "Woche" oder "Monat" wurden, die Zeitplan nur einen Tag, eine Woche oder Monat, Tag ausführen würde Hilfethemas.|
|<code>{"minutes":[15]}</code>|Führen Sie 15 Minuten nach der vollen Stunde stündlich. Ausgeführt wird jede Stunde, beginnend bei 00:15 ein 1:15 Uhr, 2:15 Uhr usw. beginnt und endet am 10:15 PM und 11:15 PM.|
|<code>{"hours":[17], "weekDays":["saturday"]}</code>|5 Uhr jeden Samstag ausführen jede Woche|
|<code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code>|5 Uhr am Montag, Mittwoch und Freitag ausführen jede Woche|
|<code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code>|Führen Sie bei 5:15 PM und 5:45 PM am Montag, Mittwoch und Freitag jede Woche|
|<code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code>|Führen Sie bei 5 Uhr und 17: 00 Uhr am Montag, Mittwoch und Freitag jede Woche|
|<code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code>|Um 5:15 Uhr, 5:45 Uhr ausführen 5:15 PM und 5:45 PM am Montag, Mittwoch und Freitag jede Woche|
|<code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code>|Führen Sie alle 15 Minuten auf Arbeitstagen|
|<code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code>|Führen Sie alle 15 Minuten auf Arbeitstagen zwischen 9: 00 und 4:45 PM.|
|<code>{"weekDays":["sunday"]}</code>|Führen Sie auf Sonntage zur Startzeit|
|<code>{"weekDays":["tuesday", "thursday"]}</code>|Führen Sie auf jeden Dienstag und Donnerstag zur Startzeit|
|<code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code>|Führen Sie auf der 28. Tag des jeder Monats (vorausgesetzt, Häufigkeit des Monats) um 6 Uhr|
|<code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code>|Um 6 Uhr am letzten Tag des Monats ausführen. Wenn Sie ein Projekt am letzten Tag des Monats ausführen möchten, verwenden Sie anstelle von Tags 28, 29, 30 oder 31-1.|
|<code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code>|Führen Sie auf der ersten und letzten Tag des Monats 6: 00|
|<code>{monthDays":[1,-1]}</code>|Führen Sie auf der ersten und letzten Tag des Monats zur Startzeit|
|<code>{monthDays":[1,14]}</code>|Führen Sie auf den ersten und vierzehnten Tag des Monats zur Startzeit|
|<code>{monthDays":[2]}</code>|Führen Sie auf der zweiten Tag des Monats zur Startzeit|
|<code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code>|Führen Sie auf der ersten Freitag des Monats um 5 Uhr|
|<code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code>|: Führen Sie auf der ersten Freitag des Monats zur Startzeit|
|<code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code>|Führen Sie auf der dritten Freitag aus Ende des Monats, monatlich zur Startzeit|
|<code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code>|Führen Sie auf der ersten und letzten Freitag des Monats um 5:15 Uhr|
|<code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code>|Führen Sie auf der ersten und letzten Freitag des Monats zur Startzeit|
|<code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code>|Führen Sie auf der fünften Freitag des Monats zur Startzeit. Wenn keine fünften Freitag in einen Monat vorhanden ist, wird dies nicht ausgeführt, da es eingeteilt nur fünften freitags ausgeführt. Sie können auch-1 anstelle von 5 für das Eintreten verwenden, wenn Sie den Auftrag auf der letzten Mausklick ausführen möchten Freitag des Monats.|
|<code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code>|Alle 15 Minuten letzten Freitag des Monats ausgeführt|
|<code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code>|Um 5:15 Uhr, 5:45 Uhr ausführen 5:15 PM und 5:45 PM am 3rd Mittwoch des Monats|

## <a name="see-also"></a>Siehe auch


 [Was ist der Scheduler?](scheduler-intro.md)

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
