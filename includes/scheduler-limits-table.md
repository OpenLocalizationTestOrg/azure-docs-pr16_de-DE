Die folgende Tabelle beschreibt die einzelnen die wichtigsten Kontingente, Grenzwerte, Standardeinstellungen und Drosselungen in Azure Scheduler.

|Ressource|Beschreibung der Grenzwert|
|---|---|
|**Größe des**|Die maximale Größe ist 16 KB. Wenn ein sich oder einem PATCH eines Auftrags größer als diese Grenzwerte ergibt, wird 400 Ungültige Anforderung Status-Code zurückgegeben.|
|**Anfordern von URL-Größe**|Maximale Größe des angeforderten URL ist 2048 Zeichen.|
|**Aggregieren Kopfzeilengröße**|Maximale aggregieren Kopfzeilengröße ist 4096 Zeichen.|
|**Anzahl der Kopfzeile**|Anzahl von maximale Kopfzeile ist 50 Überschriften.|
|**Größe des Hauptteils**|Maximale Größe des Hauptteils ist 8192 Zeichen.|
|**Serie Zeitspanne**|Maximale Serie Zeitspanne beträgt 18 Monate.|
|**Zeit zum Starten der Uhrzeit**|Maximum "Zeit, Anfangszeit" beträgt 18 Monate.|
|**Historie**|Maximale Antwort Textkörper Historie gehörende Kehrmatrix ist 2048 Byte.|
|**Häufigkeit**|Max Häufigkeit Kontingents Standard ist in einer Sammlung von kostenlosen Auftrag 1 Stunde und 1 Minute in einer Websitesammlung standard Auftrag. Die maximale Häufigkeit lässt sich auf eine Position Auflistung kleiner als der Maximalwert sein. Alle Projekte in der Auflistung Auftrag sind beschränkt auf die Position Sammlung festgelegten Wert. Wenn Sie, beim Erstellen eines Auftrags mit einer höheren Häufigkeit als die maximale Häufigkeit in der Websitesammlung Auftrag versuchen schlägt Anforderung mit einem 409 Konflikt Statuscode fehl.|
|**Aufträge**|Das standardmäßige max Aufträge Kontingent ist 5 in eine Auflistung von kostenlosen Position und 50 Einzelvorgänge in einer Websitesammlung standard Position. Klicken Sie auf eine Auflistung Auftrag lässt sich die maximale Anzahl von Aufträgen. Alle Projekte in der Auflistung Auftrag sind beschränkt auf die Position Sammlung festgelegten Wert. Wenn Sie versuchen, mehrere Aufträge als das Kontingent für die maximale Aufträge zu erstellen, schlägt die Anforderung mit einem 409 Konflikt Statuscode.|
|**Position Verlauf beibehalten**|Historie wird für bis zu 2 Monate oder bis zu der letzten 1000 Ausführungen beibehalten.|
|**Abgeschlossene und fehlerhaften Auftrag Aufbewahrungsrichtlinien**|Abgeschlossene und fehlerhafte Aufträge werden 60 Tage lang aufbewahrt.|
|**Timeout**|Es ist ein statische (nicht konfiguriert) Anforderungstimeout von 60 Sekunden für HTTP-Aktionen aus. Führen Sie für mehr laufenden Vorgänge asynchrone HTTP-Protokolle; Angenommen, eine 202 sofort zurückgegeben, aber weiterhin im Hintergrund arbeiten.|
