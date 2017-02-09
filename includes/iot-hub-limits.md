Die folgende Tabelle enthält die Grenzwerte den anderen Dienst Ebenen (S1, S2, S3, F1) zugeordnet. Informationen über die Kosten der einzelnen *Einheit* pro Ebene, finden Sie unter [IoT Hub Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

| Ressource | S1 Standard | S2 Standard | S3 Standard | F1 kostenlose |
| -------- | ----------- | ----------- | ----------- | ------- |
| Nachrichten/Tag | 400.000 | 6,000,000   | 300,000,000 | 8.000   |
| Maximale Einheiten | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Wenn Sie maximal 200 Einheiten mit einem S1 oder S2 oder S3 Ebene-Hub verwenden erwarten, wenden Sie sich an den Microsoft-Support.

In der folgenden Tabelle sind die Grenzwerte, die für Ressourcen IoT Hub gelten aufgeführt:

| Ressource | Grenzwert |
| -------- | ----- |
| Maximum Kostenpflichtiger IoT Hubs pro Azure-Abonnement | 10 |
| Maximale kostenlose IoT Hubs pro Azure-Abonnement | 1 |
| Maximale Anzahl von Gerät Identitäten<br/>  einen einzelnen Anruf zurückgegeben | 1000 |
| IoT Hub Nachricht maximale Aufbewahrungsrichtlinien für Gerät-Cloud-Nachrichten | 7 Tage |
| Maximale Größe von Gerät-Cloud-Nachricht | 256 KB |
| Maximale Größe des Blattes ein Gerät cloud | 256 KB |
| Maximale Nachrichten in Gerät Cloud Stapel | 500 |
| Maximale Größe der Nachricht Cloud-zu-Gerät | 64 KB |
| Maximale TTL für Nachrichten Cloud-zu-Gerät | 2 Tage |
| Übermittlung maximale Anzahl für Cloud-zu-Gerät <br/> Nachrichten | 100 |
| Übermittlung maximale Anzahl für Feedback Nachrichten <br/> als Antwort auf eine Nachricht Cloud-zu-Gerät | 100 |
| Maximale TTL für Feedback Nachrichten in <br/> Antwort auf eine Cloud-zu-Gerät | 2 Tage |

> [AZURE.NOTE] Wenn Sie mehr als 10 kostenpflichtiges IoT Hubs in einem Azure-Abonnement benötigen, wenden Sie sich an den Microsoft-Support.

Der Dienst IoT Hub Steuerung Anfragen aus, wenn die folgenden Kontingente überschritten werden:

| Schränken Sie | Wert pro hub |
| -------- | ------------- |
| Identität Registrierung Vorgänge <br/> (erstellen, abrufen, Liste, aktualisieren, löschen) <br/> einzelne oder Massen importieren/exportieren | 5000/min/Einheit (für S3) <br/> 100/min/Einheit (für S1 und S2). |
| Gerät Verbindungen | 6000/sec/Einheit (für die Kürzel a3), 120/sec/Einheit (für S2), 12/sec/Einheit (für S1). <br/>Mindestens 100/sec. |
| Sendet Cloud-Gerät | 6000/sec/Einheit (für die Kürzel a3), 120/sec/Einheit (für S2), 12/sec/Einheit (für S1). <br/>Mindestens 100/sec. |
| Sendet Cloud-zu-Gerät | 5000/min/Einheit (für die Kürzel a3), 100/min/Einheit (für S1 und S2). |
| Cloud-zu-Gerät empfängt. | 50000/min/Einheit (für die Kürzel a3), 1000/min/Einheit (für S1 und S2). |
| Datei hochladen Vorgängen | 5000-Datei hochladen Benachrichtigungen/min/Einheit (für S3), 100 Datei hochladen Benachrichtigungen/min/Einheit (für S1 und S2). <br/> 10000 SAS-URIs können gleichzeitig out für ein Konto Azure-Speicher sein.<br/> 10 SAS-URIs/Gerät kann sich gleichzeitig sein. |
