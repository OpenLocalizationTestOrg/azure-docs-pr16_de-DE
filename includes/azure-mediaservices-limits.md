Ressource|Standardmäßige Grenzwert|Obergrenze
---|---|---
Azure Media Services (AMS) Konten in ein einzelnes Abonnement||25
Ressourcen pro AMS-Konto||1.000.000<sup>1</sup>
Verkettete Vorgänge pro Projekt||30
Ressourcen pro Vorgang||50
Ressourcen pro Projekt||100
Stellen pro AMS-Konto ||50.000<sup>2</sup>
Eindeutige Locator gleichzeitig Zusammenhang mit einer Anlage||5<sup>4</sup>
Live-Kanäle pro AMS-Konto </p></td>|5</p></td>|N/v<sup>1</sup>
Programme beendet Zustand pro Kanal </p></td>|50</p></td>|N/v<sup>1</sup>
Programmen in den Zustand pro Kanal ausgeführt </p></td>|3</p></td>|3
Streaming Endpunkte bei der Ausführung von Bundesstaat pro AMS-Konto</p></td>|2</p></td>|N/v<sup>1</sup>
Einheiten pro streaming Endpunkt Streaming </p></td>|10 </p></td>|N/v<sup>1</sup>
Codierung Einheiten pro AMS-Konto </p></td>|25</p></td>|N/v<sup>1</sup>
Speicherkonten | |1.000<sup>5</sup>
Richtlinien || 1.000.000<sup>6</sup>

<sup>1</sup> Sie können anfordern, um die Grenzwerte für dieses Kontingent Öffnen einer Support-Ticket zu aktualisieren. Weitere AMS Konten Grenzwerte erhöhen, stattdessen Übermitteln einer Support-Ticket nicht erstellt werden.

<sup>2</sup> diese Zahl schließt in der Warteschlange, Fertig, aktiven und unterbrochen Aufträge. Es ist nicht gelöschten Aufträge enthalten. Sie können die alten Aufträge mit **IJob.Delete** oder die HTTP-Anforderung **Löschen** löschen.

<sup>3</sup> Wenn eine Anforderung für Liste Auftrag Personen vornehmen, werden maximal 1.000 pro Anforderung zurückgegeben. Wenn Sie alle gesendeten Aufträge verfolgen müssen, können Sie in [OData-System Abfrageoptionen](http://msdn.microsoft.com/library/gg309461.aspx)beschriebenen oberen/überspringen.

<sup>4</sup> Locator sind nicht für die Verwaltung von pro-Steuerung des Benutzerzugriffs konzipiert. Verwenden Sie Verwaltung digitaler Rechte (Digital Rights Management, DRM) Lösungen, die unterschiedliche Zugriffsrechte für einzelne Benutzer zu verleihen.

<sup>5</sup> die Speicherkonten müssen aus dem gleichen Azure-Abonnement.

<sup>6</sup> ist es maximal 1.000.000 Richtlinien für unterschiedliche AMS-Richtlinien (z. B. für Locator Richtlinie oder ContentKeyAuthorizationPolicy). Verwenden Sie die gleichen Richtlinien-ID, wenn Sie immer die gleiche Tage verwenden / Dateizugriffsberechtigungen / usw..
