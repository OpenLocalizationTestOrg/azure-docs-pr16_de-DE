## <a name="dynamic-service-plan"></a>Dynamische Serviceplan

In der dynamischen Dienst planen werden eine Instanz der Funktion app Ihrer apps (Funktion) zugewiesen werden. Wenn mehrere Instanzen dynamisch hinzugefügt werden soll, benötigt.
Diese Instanzen können über mehrere computing-Ressourcen, die Nutzung der Azure-Infrastruktur verfügbaren machen erstrecken. Darüber hinaus werden die Funktionen minimieren die Gesamtzeit für die Verarbeitung von Besprechungsanfragen erforderlich parallel ausgeführt. Ausführung Zeit für jede Funktion ist addierte in Sekunden an, und von der enthaltenden Funktion app aggregiert. Mit Kosten durch die Anzahl der Instanzen, deren Arbeitsspeichergröße und Ausführung Gesamtzeit gemessen in Gigabyte Sekunden leistungsgesteuert. Dies ist eine hervorragende Option, wenn Ihre Bedürfnisse berechnen wiederkehrender sind oder Ihre Position Zeiten in der Regel sehr kurz sein, wie es Ihnen ermöglicht, Zahlen nur für Ressourcen berechnen, wenn sie tatsächlich verwendet werden.   

### <a name="memory-tier"></a>Arbeitsspeicher Ebene

Abhängig von der Funktion müssen Sie die Größe des Arbeitsspeichers erforderlich, dass die Ausführung in der App-Funktion (Container Funktionen) auswählen können.
Die Größe Speicheroptionen variieren von **128 MB auf 1.536 MB**. Die Größe des ausgewählten Speichers entspricht der arbeiten Festlegen von allen Funktionen, die Teil der Funktion app sind erforderlich. Wenn der Code mehr Arbeitsspeicher als die ausgewählte Größe erfordert, wird die Funktion app-Instanz aufgrund fehlender verfügbarer Speicher beendet.

### <a name="scaling"></a>Skalierung

Die Funktionen Azure-Plattform wird der Datenverkehr muss, basierend auf der konfigurierten Trigger entscheiden, wann Sie nach oben oder unten skalieren ausgewertet werden. Die Genauigkeit der Skalierung wird die app (Funktion). Zentrales Skalieren bedeutet das in diesem Fall weitere Instanzen von einer Funktion app hinzufügen. Proportionalem wie Datenverkehr nach unten wechselt, sind Funktion app Instanzen deaktiviert-Gelegenheit Skalierung nach unten bis 0 (null), wenn keine ausgeführt werden.  

### <a name="resource-consumption-and-billing"></a>Ressourcenverbrauch und Abrechnung

In der dynamischen Modus ressourcenzuteilung anders als der standard-App-Serviceplan abgeschlossen ist, weshalb auch Verbrauch Modell anders ab, gleicht für ein Modell "Bezahlung pro Einsatz" steht. Verbrauch wird pro Funktion app nur für Zeit gemeldet, wenn Code ausgeführt wird.  
Es wird durch Multiplizieren die Größe des Speichers (in GB) durch die Gesamtmenge der Ausführung (in Sekunden) für alle Funktionen, die innerhalb dieser Funktion app berechnet. Die Maßeinheit von Verbrauch werden **GB-s (Gigabyte Sekunden)**.